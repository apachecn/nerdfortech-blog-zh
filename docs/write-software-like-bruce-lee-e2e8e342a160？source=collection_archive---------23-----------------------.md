# 像李小龙一样写软件

> 原文：<https://medium.com/nerd-for-tech/write-software-like-bruce-lee-e2e8e342a160?source=collection_archive---------23----------------------->

## 李把他的截拳道称为“用最少的动作和能量直接表达自己的感情”。截拳道背后的一个主要思想是，武术家应该尽量减少动作的数量和范围，专注于以闪电般的速度提供最大的力量。

![](img/8998bcc73ee8b8dcda1053253d283577.png)

由[帕布鲁·托拉多](https://unsplash.com/@pablotorrado?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

20 世纪 70 年代是功夫电影的黄金时代，李小龙主演的功夫电影名列其中。他对自己系统的奉献导致了他对完善“一英寸”出拳的渴望——只有一英寸移动的出拳可能会造成重大创伤。他也有点像哲学家——在他的道场外面，据说有一块墓碑上刻着

> 这里躺着一个曾经流动的人的记忆，被经典的混乱塞满和扭曲

我认为，如果 Bruce 是一名程序员，他将会是敏捷开发实践的狂热爱好者，或者像一些人所说的“敏捷”。把你的努力集中在短时间内，要灵活，不要被传统的混乱所阻碍。

我跑题了，只有一点点。读者可能已经听说过我花了九个月时间开发一个网络应用程序。这是一个类似的故事，但从一个令人眼前一亮的应用程序变成一个改变游戏规则的应用程序只需要六周时间。不可否认，一个应用程序只对一个亚文化的很小的子集感兴趣，但这仍然是事实。

这个叙述也是从 11 月 5 日到 12 月 16 日(第二次)公开发布之间的笔记和 git 日志中重建的。这是一个在现实生活的开发场景中使用 Elm 的故事，但更能说明问题的是，我认为它阐明了敏捷在变成黑客之前是多么敏捷。30 天内大约 260 次提交；相当紧张，但总是在控制之下。这是没有稳定器的软件开发。

11 月 5 日，不知从哪里冒出来的，笔记本里有一张 [GPXmagic](http://stepwiserefinement.co.uk.s3-website-eu-west-1.amazonaws.com/GPXmagic/index.html) UI 的小草图。这当然是可以辨认的，而且从那以后也没有太大的变化。在对页上，有一条关于使用样条曲线平滑直线段之间的急转弯的注释。(样条线在这个版本中被放弃，但在后来的 2.0 版本中惊人地卷土重来。)

在此之前，很久以前，就有一些东西；它只是在成为一个“项目”之前没有制作笔记本。我在为链家搜索 WebGL 的时候，发现了`[elm-3d-scene](https://package.elm-lang.org/packages/ianmackenzie/elm-3d-scene/latest/Scene3d)`库。这个惊人的库抽象了 WebGL 着色器。它让用户组成一个几何形状，位置灯和摄像机的三维模型，并负责渲染。它在概念上令人惊叹，并在 pure Elm 中实现。令人羞愧。

我当时想知道这是否有助于 CRT 追踪，并得出结论，它不会。我将不得不为轨迹中的任何变化，包括高频噪声，重新组成“网格”。包装上的说明清楚地表明，这种构图是昂贵的，应该不经常进行，而移动相机(比如说)是一种非常便宜的操作。但是它一直伴随着我，我想我已经准备好利用任何机会来使用它。

然后我进入了 [RGT](https://www.rgtcycling.com) ，室内自行车模拟器。特别是他们对“神奇道路”的支持，允许用户上传任何 GPX 路线或骑行，并在他们的模拟器中渲染，以便可以用室内智能教练设置骑行。不幸的是，大多数原始的 GPX 文件都充满了错误和小故障，所以魔法之路可能是不可逾越的。它们必须首先被平滑。主流的编辑工具看起来都很笨重，晦涩难懂，而且很难想象各种工具的效果。

一个想法的萌芽开始形成。我可以用 3D 渲染和编辑 GPX 路线吗？我能成为一名更好的 GPX 编辑吗？

进展的第一个证据是 2020 年 11 月 9 日的首次 git 提交。这是一个基本的 Elm 程序，允许读者打开一个 CSV 文件并在屏幕上显示其内容。这几乎不是火箭科学，但却是一个很好的起点，因为我以前没有做过任何文件 I/O，如果没有别的事情，我们将会读写 GPX 文件。

让我们把整个**清单放在这里，省略空白和导入语句，只是为了说明 Elm 有多简单。**

```
module Main exposing (main)import Browser
import File exposing (File)
import File.Select as Select
import Html exposing (Html, button, p, text)
import Html.Attributes exposing (style)
import Html.Events exposing (onClick)
import Taskmain : Program () Model Msg
main =
 Browser.element
 { init = init
 , view = view
 , update = update
 , subscriptions = subscriptions
 }type alias Model =
 { csv : Maybe String
 }init : () -> (Model, Cmd Msg)
init _ =
 ( Model Nothing, Cmd.none )type Msg
 = CsvRequested
 | CsvSelected File
 | CsvLoaded Stringupdate : Msg -> Model -> (Model, Cmd Msg)
update msg model =
 case msg of
 CsvRequested ->
 ( model
 , Select.file [“text/gpx”] CsvSelected
 )CsvSelected file ->
 ( model
 , Task.perform CsvLoaded (File.toString file)
 )CsvLoaded content ->
 ( { model | csv = Just content }
 , Cmd.none
 )view : Model -> Html Msg
view model =
 case model.csv of
 Nothing ->
 button [ onClick CsvRequested ] [ text “Load CSV” ]Just content ->
 p [ style “white-space” “pre” ] [ text content ]subscriptions : Model -> Sub Msg
subscriptions _ =
 Sub.none
```

和链家 app 一样，我既不提供解释，也不提供理由。不过，也不尽然。我将仅仅指出，Elm 程序中的每一件有意义的事情都发生在`update`中，这是来自不纯洁的外部世界的消息的结果。这导致了状态的新版本(在模型中)，并且可能导致一些命令在不纯的外部世界中被执行(这里，显示一个文件打开对话框并加载一个文件)。

## 论规划

让我们制定一个计划。问题域是什么？

-解析 GPX 文件
-显示 GPX 音轨
-应用编辑以产生更平滑的音轨，而不会太复杂

我们可以使用吉拉，特雷罗或类似的，但我们可以留在 IDE 中，把 TODOs 放在代码中。这些可以在 IDE 的 TODO 窗格中列出，因此我们可以将它们放在代码中的任何位置，以标记意图或技术债务，并在以后轻松找到它们。

```
*--TODO: Use elm-ui
--TODO: Optional tabular display of track points
--TODO: Track points in elm-3d-scene
--TODO: Create road segments
--TODO: Road segments in elm-3d-scene
--TODO: Colour by gradient
--TODO: Camera rotation
--TODO: Fly-through
--TODO: Toggle display elements
--TODO: Dark mode*
```

抱歉，以下是前 243 次提交，按顺序排列。这是展示变化速度的最佳方式。

```
*09/11/2020, 15:20 : Initial commit
09/11/2020, 15:52 : csv -> gpx. Nothing clever.
09/11/2020, 17:07 : regex is promising for quick parser.
09/11/2020, 18:02 : Parses.
09/11/2020, 19:07 : Minus signs!
09/11/2020, 22:19 : Bounding box.
10/11/2020, 13:10 : TODOs and start logic for 3d display.
10/11/2020, 14:03 : Switch to elm-ui.
10/11/2020, 14:30 : Display track name from GPX.
10/11/2020, 15:34 : Scaling to [-1, +1] drawing coordinates.
10/11/2020, 15:37 : Fix z scaling. 'cos ele is in metres not degrees.
10/11/2020, 15:50 : First point cloud in elm-3d-scene!
10/11/2020, 15:54 : Adjust viewpoint. Hillingdon looks OK.
10/11/2020, 16:05 : Rotate with mouse (!).
10/11/2020, 16:45 : Contruct track on file load, not in view.
10/11/2020, 16:52 : Move parsing work out of update (textually).
10/11/2020, 16:59 : See where the ground is.
10/11/2020, 17:10 : Update TODOs.
10/11/2020, 18:12 : Added road segments but width is not right yet.
10/11/2020, 18:52 : Road segments draw OK with offsets.
10/11/2020, 21:59 : Prep for flythourgh - road segment numbers.
11/11/2020, 12:18 : Added HTTP load of GPX.
11/11/2020, 13:42 : View of the road ahead. No movement yet.
11/11/2020, 17:37 : Acceptable manual flythrough.
11/11/2020, 18:11 : With background and pillars that hold the road up.
11/11/2020, 20:43 : First published version.
12/11/2020, 12:34 : TODOs.
12/11/2020, 12:35 : Remove unused imports.
12/11/2020, 15:47 : Cones to make pillars meet the road.
12/11/2020, 16:15 : Added segment detail display. Corrected bearing.
12/11/2020, 17:02 : Added buttons for advance and retreat.
12/11/2020, 21:13 : Tweaking the first person view.
13/11/2020, 11:56 : Distances accurate now. Summary view. Rounded to metres.
13/11/2020, 12:34 : Add number formatting.
13/11/2020, 13:32 : Fix some bugs. Use Array for quicker access in views.
13/11/2020, 13:44 : Wrap around track when incrementing.
13/11/2020, 16:55 : Latest build output.
13/11/2020, 18:52 : New view, not yet zoomable.
13/11/2020, 20:02 : Don't need current node & segment.
13/11/2020, 20:15 : Zoomable but mouse move also rotates!
13/11/2020, 22:28 : Mouse capture, but a tad sensitive!
13/11/2020, 22:52 : Rotate and zoom better, but zoom range needs thinking through.
14/11/2020, 09:27 : Nice logarithmic zoom.
14/11/2020, 11:46 : Use 'sunny' scene consistently.
14/11/2020, 11:58 : All views zoomable.
14/11/2020, 12:58 : Nice gradient color drops.
14/11/2020, 13:59 : Stop saving build output.
14/11/2020, 13:59 : Merge remote-tracking branch 'origin/master'
14/11/2020, 14:38 : User can select display elements.
14/11/2020, 14:52 : Added some about text on Options view.
14/11/2020, 15:32 : Purple cones point at abrupt (10%) gradient changes.
14/11/2020, 16:14 : Some tidy up of view behaviour. Now need layout tweaks.
14/11/2020, 16:57 : Making the spacing more consistent.
14/11/2020, 18:18 : Adjustable gradient change threshold.
14/11/2020, 18:53 : Each mode separately zoomable.
14/11/2020, 21:00 : Disc shows the focal segment in third person view.
15/11/2020, 12:34 : Rather crude tabular problem list with direct navigation.
15/11/2020, 12:51 : Prepare for different problem types.
15/11/2020, 14:26 : Detects zero length segments and sudden gradient changes seperately.
15/11/2020, 15:39 : Problems detected and reported Captain.
15/11/2020, 16:11 : Cosistent index from zero.
15/11/2020, 16:33 : Corrected bearing difference calculation. Not prettily.
15/11/2020, 17:34 : Broke up the parse function, which did far too much.
15/11/2020, 18:20 : Deleted a trackpoint. Not saving GPX yet!
15/11/2020, 20:43 : Check for problem after loading!
15/11/2020, 21:04 : Prepare for XML creation.
15/11/2020, 21:40 : Writes GPX but it doesn't read back in.
16/11/2020, 08:56 : Can now read output back in. Number format was using a unicode -ve sign.
16/11/2020, 09:21 : Highlight selected problem in problem lists.
16/11/2020, 11:25 : Fix XML. TODO prioritisation.
16/11/2020, 11:42 : Display file name.
16/11/2020, 12:04 : Output file name is input file name with date time appended.
16/11/2020, 13:12 : Prepare for gradient smoothing.
16/11/2020, 15:30 : Gradient smoothing.
16/11/2020, 15:37 : Tighten up text in gradient problems.
16/11/2020, 15:47 : Keep those eyes level!
16/11/2020, 15:48 : TODO done.
17/11/2020, 08:40 : Preserve view settings over fix.
17/11/2020, 12:15 : Much tider problem list. Details are not really needed.
18/11/2020, 15:01 : Added very simplistic and laborious maths for finding incircles.
18/11/2020, 15:28 : Show gradient problems directly in fixing pane.
18/11/2020, 15:44 : Prep for working out the bend incircle.
18/11/2020, 17:05 : Close but not cigar. We may have the start and end points mixed up.
18/11/2020, 17:40 : Remove data from bearing change error list.
18/11/2020, 18:40 : Corrected minor errors in Geometry101.elm.
18/11/2020, 21:08 : Can move dropped marker incrementally.
18/11/2020, 22:15 : Group box around navigation controls.
19/11/2020, 11:27 : Single Save button at page top.
19/11/2020, 13:51 : Substantial break up of the HUGE 'derive' functions prior to implementing bend smoothing.
19/11/2020, 14:30 : Still recfactoring. Need to makes Nodes and Roads for the bypass, then Entities.
19/11/2020, 17:30 : FINALLY found problem which was my matrix determinant cut-off.
20/11/2020, 09:25 : Reoragnise pages. Simpler.
20/11/2020, 10:35 : Adjust effect of gradient smoothing.
20/11/2020, 10:40 : Better undo message.
20/11/2020, 10:44 : Update TODO.
20/11/2020, 11:09 : TESTED for release.
20/11/2020, 11:21 : About text update.
20/11/2020, 11:42 : TODOs.
20/11/2020, 13:28 : Quick fix for filename. Put gpx at end, after shortened timestamp.
20/11/2020, 17:27 : First person fly through.
20/11/2020, 17:58 : Flythrough in third person, but doesn't work well.
20/11/2020, 18:02 : Nicer plain colour curtain.
20/11/2020, 18:03 : Stop flythrough when loading new file.
21/11/2020, 10:31 : 2020-11-21 release. Flythrough mainly.
21/11/2020, 13:08 : Remove gradient slider from Options.
21/11/2020, 13:10 : Label on curtain style buttons.
21/11/2020, 14:06 : Single node chamfer for gradient changes.
21/11/2020, 16:46 : TODO priorities.
21/11/2020, 17:29 : Beginning to modularise.
21/11/2020, 17:56 : Some modularisation. Still works.
21/11/2020, 21:34 : Moved all the visual entity creation out of Main.
21/11/2020, 22:01 : Tidier module.
22/11/2020, 08:51 : ToDos get their own file.
22/11/2020, 12:05 : Generate visual elements as needed instead of creating them all each time anything changes.
22/11/2020, 12:09 : Moving functions into their new homes.
22/11/2020, 12:30 : Pastel curtain option.
22/11/2020, 13:10 : Factor out mapping to clipspace ready for profile.
22/11/2020, 14:39 : Drawing with replacable node mappers.
22/11/2020, 15:09 : Road unrolling OK. Need new view now.
22/11/2020, 15:49 : First rendering of profile. Not too bad, not done.
22/11/2020, 17:05 : Empirically working towards good scaling for profile view.
22/11/2020, 17:50 : Just keep the unrolled roads in the Model. Not clever. Workable.
22/11/2020, 18:33 : Out by one error selecting current node for profile view.
23/11/2020, 09:07 : Profile looks better. Still lacking markers.
23/11/2020, 09:32 : Marked nodes in Elevation view. Good to go now I think.
23/11/2020, 09:38 : Use cones for marking nodes.
23/11/2020, 09:50 : Fly through controls only seen in 1st and 3rd person views.
23/11/2020, 09:54 : About text for new drop.
23/11/2020, 12:29 : Smartened up flythrough code and it now lives in the right module.
23/11/2020, 13:37 : Basic Plan view.
23/11/2020, 14:32 : Starting to revive bend smoother.
23/11/2020, 17:30 : Close to getting bends better. Need to watch out for angle 'wrap'.
23/11/2020, 18:19 : Buggy but saving for rollback.
24/11/2020, 10:19 : Still can't figure out arcs.
24/11/2020, 12:37 : So many silly mistakes. Arc2d does the trick.
24/11/2020, 13:02 : Ready to implement the smoothing.
24/11/2020, 16:23 : Bend smoothing is in, but minor bugs.
24/11/2020, 16:39 : Track points needed to be re-indexed!
24/11/2020, 17:05 : About and TODO updates.
25/11/2020, 11:17 : Chamfering tries to use 4m of each segment. More possiblity of applying multiple times.
25/11/2020, 12:41 : Foot wide gradient coloured line on road, optionally.
25/11/2020, 18:09 : TODOs, and 2020-11-25 release.
26/11/2020, 13:57 : 2020-11-26 1.0.0
27/11/2020, 14:41 : Less fugly code for Bend Smoother, ready to accept new cases.
27/11/2020, 16:31 : Does something reasonable with dirergent segments.
27/11/2020, 17:57 : Removed WIP marks. Update TODO.
28/11/2020, 09:14 : Remove unused 'roadmapper' concept. Fixed mapper in place.
28/11/2020, 09:23 : Prep for terrain work.
28/11/2020, 10:38 : Terrain experiment successful.
28/11/2020, 12:09 : Terrain extends outside the track.
28/11/2020, 13:13 : Imperfect but encouraging terrain.
28/11/2020, 16:26 : Not bad. Except where there are tight hairpins, which will always be problematic.
28/11/2020, 17:06 : With better rendering.
28/11/2020, 17:23 : Matte road surface. Updated TODOs.
28/11/2020, 21:37 : Build terrain on explicit request only.
29/11/2020, 16:24 : Unused argument in Flythrough removed.
29/11/2020, 16:37 : Slightly smoother rotate into bends.
02/12/2020, 17:53 : De-dupe vertex list, and some code tidy up seems to make Terrain more reliable.
03/12/2020, 09:46 : Insert node before or after, and also from Bend Fixer.
03/12/2020, 16:18 : Closes loop.
04/12/2020, 17:39 : Better loop closure adds extra end trackpoint so that smoothers can be applied.
04/12/2020, 17:49 : Oops. Wrong order.
04/12/2020, 18:20 : Reverted back to old loop closure.
05/12/2020, 12:49 : Minor tidy up of Undo code.
05/12/2020, 12:56 : Undo restores the then-current current and marked nodes.
05/12/2020, 13:07 : Or, without dodgy state bug.
05/12/2020, 13:17 : Reinstate better loop closure.
05/12/2020, 13:39 : Reposition markers when applying smoothing.
05/12/2020, 16:05 : Redundant definition of Flythrough.
05/12/2020, 19:10 : No more metersToClipSpace! elm-3d-scene takes care of scaling for us.
05/12/2020, 20:42 : Tidy up after removal of scale factor.
06/12/2020, 16:58 : Rework of geometry. Compiles but is wrong.
06/12/2020, 17:36 : Removed ScalingInfo.
06/12/2020, 17:56 : Just need to fix terrain. Edges are MIA.
06/12/2020, 18:26 : Terrain is back. Flythrough reset broked. Elevation broked.
06/12/2020, 18:28 : Flythrough reset OK.
06/12/2020, 18:31 : Elevation OK. I'd turn the options off. Maybe that shouldn't happen.
06/12/2020, 18:37 : TODOs.
06/12/2020, 19:19 : Tidy up try bend smoother.
06/12/2020, 22:44 : Bend smoothing restored.
07/12/2020, 12:36 : Correct elevation marker placement.
07/12/2020, 12:45 : Pre for release.
07/12/2020, 17:20 : Limit number of segments in some bend smoothing cases.
08/12/2020, 04:19 : Improved elevation spread over smoothed bends with many segments added.
08/12/2020, 11:08 : Straightener.
08/12/2020, 14:03 : Node nudger.
08/12/2020, 15:33 : Road split.
08/12/2020, 15:45 : About text updated.
08/12/2020, 18:28 : Bend smoother line above road surface.
08/12/2020, 18:35 : Fix marker positioning after bend smoothing.
08/12/2020, 19:12 : Fix flythrough clipping plane.
08/12/2020, 19:16 : TODO update.
09/12/2020, 11:34 : Fix silly error in straightener.
09/12/2020, 12:35 : Fix silly error in bend smoother.
09/12/2020, 12:36 : Update about text for release.
09/12/2020, 17:51 : Redundant constant removed.
09/12/2020, 19:55 : TODOs.
10/12/2020, 12:39 : Nudging any which way.
10/12/2020, 12:44 : Update About text for release.
10/12/2020, 21:04 : Transitioning to new UI with more compact tools layout.
11/12/2020, 11:59 : Looking smarter. Need to sort out the details of each control.
11/12/2020, 12:22 : Gradient fix pane improved.
11/12/2020, 12:41 : Nudge panel better but vertical scroller won't centre.
11/12/2020, 12:48 : Bend smoother pane done but serious bug when smoothing at start.
11/12/2020, 13:54 : Fix stupid error when bend smoothing at route start.
11/12/2020, 14:07 : Straighten pane sorted.
11/12/2020, 15:13 : All the tool panes in the right places.
11/12/2020, 16:02 : Added track point delete.
11/12/2020, 16:20 : Move loop start point.
11/12/2020, 16:27 : Prep for autosave.
11/12/2020, 16:32 : Finessing settings after file load.
11/12/2020, 16:46 : Release ready barring text.
11/12/2020, 17:25 : About text for release.
12/12/2020, 09:06 : Redo button.
12/12/2020, 10:48 : Limit to 10 undo actions.
12/12/2020, 11:00 : Reverse track.
12/12/2020, 11:57 : Essence of map.
12/12/2020, 12:08 : Disable map test for now.
12/12/2020, 16:11 : Minor big fixes.
12/12/2020, 16:13 : Local dummy API key.
12/12/2020, 22:23 : Web Mercator projection.
13/12/2020, 11:53 : Reports input errors. Clear old model properly on new file.
13/12/2020, 12:02 : Prep for bug fix release.
13/12/2020, 12:57 : Progress on map overlay.
13/12/2020, 14:13 : Map test. 
13/12/2020, 15:44 : Seems I should have this in git.
13/12/2020, 17:52 : End of mapping play, for a while.
14/12/2020, 12:59 : Uses ports to display a map. Some work required!!!
14/12/2020, 15:56 : Flythrough show distance from start.
15/12/2020, 16:32 : Mapping with bugs.
16/12/2020, 11:12 : Map layout I can almost live with.
16/12/2020, 12:13 : We have track on map, though it doesn't change when route changes.
16/12/2020, 12:27 : Can switch map but track needs prompting.
16/12/2020, 13:41 : Map switches when file loaded.
16/12/2020, 13:54 : Removing temporary key.
16/12/2020, 14:04 : Avoiding runtime errors in JS.
16/12/2020, 14:26 : Prep for release.
16/12/2020, 14:31 : Missed a couple of alignTops there.
16/12/2020, 14:42 : Back to life as normal after map diversion.
16/12/2020, 14:44 : Minor about text changes.
16/12/2020, 16:44 : Simple static map image.
16/12/2020, 16:49 : 2020-12-16-release with simple static map.
16/12/2020, 17:38 : Use Feather icons. Nice and avoids unicode issues.*
```

我不打算一一介绍。基本上，那几周过得很棒。11 月 10 日，GPX 赛道点首次呈现 3D 效果。

![](img/9fc5cf423370fe258295d91dd65b8b24.png)

12 月 16 日的发布会是什么样的？一个功能齐全，看起来相当聪明的 GPX 编辑器。

![](img/05b38c31d770badd46861a131cd02fa5.png)

## 什么管用

这整个练习是对在 Elm 中开发一个重要的 web 应用程序的巨大支持。语言从来不会打断开发者的流程。是的，你必须处理编译错误，但这也是乐趣的一部分。想把数据结构从列表改为数组吗？只需更改定义，然后修复所有编译错误；会有用的。

即使有一个宽松的计划，也被证明是绝对重要的。也有饭桶。从日志中可以清楚地看到我犯错误的频率。让你在做出改变时更加自信，尽管我在这方面还是个新手。

带有 Elm 插件的 IntelliJ IDEA 创造了一个非常可爱的开发环境。

MapBox 地图组件非常灵活。

## 什么不好

我没有自动化测试。我如何“测试”一个三维轨道渲染还远不清楚，更不用说轨道编辑的效率或准确性了。

MapBox 组件需要一些 JavaScript 来集成，坦白地说，这是一个 PITA。它还破坏了 elm-ui 布局，因此需要多层“包装”。这在第二版中才真正得到解决。

## 布鲁斯会怎么做？

我以参考李小龙和截拳道开始这篇文章。这不仅仅是点击诱饵。我觉得我已经用最少的动作和能量达到了*直接表达一个人的感情了吗*？是的，实际上。语言和工具可以做到这一点，当然，我不需要会议、评审、批准、仪式和所有阻碍敏捷变得足够敏捷的包袱。

## 它能为团队工作吗，它能扩展吗？

我要大胆的说，是的。太多的“敏捷”仅仅是对仪式的坚持。有迷失目标的真正危险。我将不得不写更多这方面的东西，但它应该是关于建立工作能够*流动的环境*。你需要自己重新发现这一点:透过仪式、公告板、工具(或其背后),把它们仅仅看作是使能器，而不是用来收报机的盒子。