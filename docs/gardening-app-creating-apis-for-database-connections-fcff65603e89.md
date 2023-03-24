# 园艺应用程序:为数据库连接创建 API

> 原文：<https://medium.com/nerd-for-tech/gardening-app-creating-apis-for-database-connections-fcff65603e89?source=collection_archive---------12----------------------->

试图让 Buildozer、SQLALchemy 和一个数据库驱动程序在 Android 上工作已经成为一个死胡同。有几个问题，包括 Postgres 的一个基本模块(psycopg2)由于某种原因与 Buildozer 不兼容。相反，我使用 Flask 创建自己的 API，这样我的 Kivy 应用程序只需发送一个 web 请求，而不是直接与数据库交互。我将使用 Heroku Postgres 来托管数据库，目前我正在使用 Postman 测试我的 API。我为以下内容创建了单独的 API:

*   搜索植物——这要用到两次:显式植物搜索和花园页面。每次用户查看一个特定的花园时，他们将看到该花园中的植物列表。
*   为主页生成一个花园列表，用户可以在主页上看到他们所有的花园。
*   添加工厂—可在两个位置访问:主页和工厂搜索页面。
*   添加花园—从主页访问。
*   更新工厂—在查看单个工厂的完整信息时访问。
*   更新花园-从花园页面访问。
*   删除花园-从花园的页面访问。
*   删除工厂-查看单个工厂的完整信息时访问。

代码如下:

```
from flask import Flask, request
from sqlalchemy import MetaData, create_engine, Table, Column, String, Integer, and_
from sqlalchemy.sql.expression import Update
import json

app = Flask(__name__)

meta = MetaData()
engine = create_engine("postgresql+psycopg2:// , echo=True)
connection = engine.connect()
my_plants = Table(
    "plants", meta,
    Column("id", Integer, primary_key=True),
    Column("name", String(length=50), ),
    Column("common_name", String(length=50)),
    Column("category", String(length=50)),
    Column("location", String(length=50)),
    Column("year", Integer),
    Column("notes", String(length=500)),
)
my_gardens = Table(
    "gardens", meta,
    Column("id", Integer, primary_key=True),
    Column("name", String(length=50)),
    Column("notes", String(length=250)),
)
meta.create_all(engine)

@app.route("/gardens/create", methods=["POST"])
def create_garden():
    name = request.form['name']
    notes = request.form['notes']
    new_garden = [
        {"name": name,
         "notes": notes}
    ]
    connection.execute(my_gardens.insert(), new_garden)
    return ''  # --- I needed a return statement, even though this API doesn't return anything to the app --- #

@app.route("/plants/create", methods=["POST"])
def create_plant():
    name = request.form["name"]
    common_name = request.form["common_name"]
    category = request.form["category"]
    location = request.form["location"]
    year = request.form["year"]
    notes = request.form["notes"]
    new_plant = [
        {"name": name,
         "common_name": common_name,
         "category": category,
         "location": location,
         "year": year,
         "notes": notes}
    ]
    connection.execute(my_plants.insert(), new_plant)
    return ""

@app.route("/gardens/update/<int:garden_id>", methods=["UPDATE"])
def update_garden(garden_id):
    updated_name = request.form['updated_name']
    updated_notes = request.form['updated_notes']
    updated_garden_info = Update(my_gardens).where(my_gardens.columns.id == garden_id).values(name=updated_name, notes=updated_notes)
    connection.execute(updated_garden_info)
    return ""

@app.route("/plants/update/<int:plant_id>", methods=["UPDATE"])
def update_plant(plant_id):
    updated_name = request.form["updated_name"]
    updated_common_name = request.form["updated_common_name"]
    updated_category = request.form["updated_category"]
    updated_location = request.form["updated_location"]
    updated_year = request.form["updated_year"]
    updated_notes = request.form["updated_notes"]
    updated_plant_info = Update(my_plants).where(my_plants.columns.id == plant_id).values(name=updated_name, common_name=updated_common_name, category=updated_category, location=updated_location, year=updated_year, notes=updated_notes)
    connection.execute(updated_plant_info)
    return ""

@app.route("/plants/delete/<int:plant_id>", methods=["DELETE"])
def delete_plant(plant_id):
    deletion = my_plants.delete().where(my_plants.columns.ids == plant_id)
    connection.execute(deletion)
    return ""

@app.route("/gardens/delete/<int:garden_id>", methods=["DELETE"])
def delete_garden(garden_id):
    deletion = my_gardens.delete().where(my_gardens.columns.ids == garden_id)
    connection.execute(deletion)
    return ""

@app.route("/plants/search", methods=["POST"])
def search_plants():
    name = request.form["name"]
    common_name = request.form["common_name"]
    category = request.form["category"]
    location = request.form["location"]
    year = request.form["year"]
    notes = request.form["notes"]
    filters = []
    if name != "":
        filters.append(my_plants.columns.name.contains(name))
    if common_name != "":
        filters.append(my_plants.columns.common_name.contains(common_name))
    if category != "":
        filters.append(my_plants.columns.category == category)
    if location != "":
        filters.append(my_plants.columns.location == location)
    if year != "":
        filters.append(my_plants.columns.year == year)
    if notes != "":
        filters.append(my_plants.columns.notes.contains(notes))
    search = my_plants.select().where(and_(*filters))
    results = connection.execute(search).fetchall()
    return json.dumps([list(x) for x in results])

@app.route("/gardens", methods=["GET"])
def all_gardens():
    info = my_gardens.select()
    results = connection.execute(info).fetchall()
    return json.dumps([dict(x) for x in results])
```