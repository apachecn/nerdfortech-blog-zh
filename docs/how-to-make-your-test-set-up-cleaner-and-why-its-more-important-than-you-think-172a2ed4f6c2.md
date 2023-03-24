# 如何让你的测试设置更整洁，为什么它比你想象的更重要

> 原文：<https://medium.com/nerd-for-tech/how-to-make-your-test-set-up-cleaner-and-why-its-more-important-than-you-think-172a2ed4f6c2?source=collection_archive---------5----------------------->

我一直认为，如果你认为测试仅仅是为了构建健壮的软件组件和发现错误，那么你就有点误解了。他们比那更强大。

测试是一种由来已久的交流形式，也是你的公司中唯一真正可靠的文档形式。一个工具，可以围绕边缘案例建立清晰度，探索假设情景，并帮助提高效率，这是迄今为止你的公司最昂贵和最受欢迎的资源，在你为了更好的薪水而离开多年后。

但是令人沮丧的是，测试设置可能会令人困惑、冗长和不清楚，混淆了很容易成为对应用程序行为方式的集体理解的巨大资产的东西。

# 代码

出于演示目的进行了简化

```
public void validatePet(Pet pet, PetRequest petRequest) {
    validateAge(pet, petRequest.getMaxAgeInMonthsExpectation());
    validateColor(pet, petRequest.getColorExpectation());
    validateCuteness(pet, petRequest.getCutenessExpectation());
    validateTail(pet.getTail(), petRequest.getTailRequest());
}

void validateAge(Pet pet, Integer maxAgeExpectation){
    if(pet.getAgeInMonths() > maxAgeExpectation){
        throw new TooOldException();
    }
}

void validateColor(Pet pet, String color){
    if(!pet.getColor().equals(color)){
        throw new WrongColorException();
    }
}

void validateCuteness(Pet pet, Integer minCutenessExpectation){
    if(pet.getCuteness() < minCutenessExpectation){
        throw new NotCuteEnoughException();
    }
}public void validateTail(Tail tail, TailRequest tailRequest){
    if(!tail.getTailFluff().equals(tailRequest.getTailFluff())){
        throw new TailFluffException(tail.getTailFluff(), tailRequest.getTailFluff());
     }    
    if(tail.getTailLength() < 0) {
        throw new InvalidTailLengthException(tail.getTailLength());
     }    
    if(tail.getTailLength() < tailRequest.getTailLength()){
        throw new TailLengthException(tail.getTailLength(), tailRequest.getTailLength());
    }
}
```

# 测试

测试设置通常是这样的。

```
@Test
void validationFailsWhenPetNotCuteEnough() {
    // difficult to spot straight away which field has an effect on the test
    var pet =
        Pet.*builder*()
            .petType(PetType.*DOG*)
            .ageInMonths(12)
            .color("brown")
            .cuteness(2)
            .healthRating(10)
            .tail(Tail
                .*builder*()
                .tailLength(10)
                .tailFluffiness(TailFluffiness.*SUPER_FLUFFY*)
                .build())
            .build();

    var petRequest = PetRequest
        .*builder*()
        .petType(PetType.*DOG*)
        .maxAgeInMonthsExpectation(120)
        .colorExpectation("brown")
        .cutenessExpectation(8)
        .healthExpectation(8)
        .tailRequest(
            TailRequest
                .*builder*()
                .tailFluffiness(TailFluffiness.*SUPER_FLUFFY*)
                .tailLength(10)
                .build())
        .build();

*assertThrows*(NotCuteEnoughException.class,
        () -> petValidatorService.validatePet(pet, petRequest));

}
```

虽然这绝对是一个正确的测试，并且显示了验证器的行为。这并不漂亮。实际上，哪个字段与测试失败相关并不明显。是的，虽然你不需要成为一个天才就能想出来，但它确实会让我眯着眼睛，头向前翘了四五秒钟才想出来。虽然您可能会争论异常的名称和显示名称的内容是否给了您所需要的所有信息，但是根据我的经验，依赖开发人员的意愿来命名通常是不好的。

将这样的测试设置增加到成千上万的测试和比小狗更复杂的领域，它很容易成为一个令人困惑和厌倦的游戏，沃利在哪里。

因此，在我看来，这样设置的测试没有履行测试的非常重要的责任，即*创造清晰*。

此外，如果这个对象在代码库中构建了 100 次，突然我们**添加了一个大小验证器**会怎么样？是的，没错，你需要改变 100 个类。

所以让我们尝试一些不同的东西。

# **对象母模式**

从测试的角度来看，object mother 类为我们对这只可怜的小狗不切实际的期望创建了一个更加清晰的解释。然而，像许多母亲一样，随着对象母亲的成熟，它会变得抗拒变化和高维护性，需要许多测试默认对象来适应您的所有需求。下面就来看看吧。

```
public class PetObjectMother {
public static Pet getNotSoCuteDog() {
    return Pet.*builder*()
        .petType(PetType.*DOG*)
        .ageInMonths(12)
        .color("brown")
        .cuteness(2)
        .healthRating(10)
        .tail(*getLongAndFluffyTail*())
        .build();
}

public static Pet getOldDoggo() {
    return Pet.*builder*()
        .petType(PetType.*DOG*)
        .ageInMonths(120)
        .color("brown")
        .cuteness(8)
        .healthRating(10)
        .tail(*getLongAndFluffyTail*())
        .build();
 }
}public class TailObjectMother {
    public static Tail getLongAndFluffyTail(){
        return Tail.*builder*()
            .tailFluffiness(TailFluffiness.*SUPER_FLUFFY*)
            .tailLength(10)
            .build();
    }
}@Test
void validationFailsWhenPetNotCuteEnough() {
    var pet = *getNotSoCuteDog*();

    var petRequest = *getFairlyDemandingPetRequest*();

    *assertThrows*(NotCuteEnoughException.class,
        () -> petValidatorService.validatePet(pet, petRequest));
}

@Test
void validationFailsWhenPetIsTooOld() {
    var pet = *getOldDog*();

    var petRequest = *getFairlyDemandingPetRequest*();

    *assertThrows*(TooOldException.class,
        () -> petValidatorService.validatePet(pet, petRequest));
}
```

我相信您可以想象这种解决方案的可伸缩性有多差。您最终需要为每个测试用例使用一个新的方法，并且会很快膨胀。

对此，一个快速而明显的解决方案是将默认对象替换为一种接受参数的默认对象，但这可以很快变成一个普通的测试类，而没有构建器模式的清晰性(如果您确实选择使用构建器模式)。

此外，尽管这个解决方案在客户端看起来更漂亮，但它实际上并没有创造出多少清晰度。我们仍然需要依靠方法的名称来检测 d to 的哪一部分影响了测试的结果。很容易被误命名或过时。

如果公司有使用测试人物角色的强大文化，这种模式可以很好地工作，这是我认可的，但是很难减轻伸缩问题，老实说，虽然我发现人物角色是一个好主意，但在我的职业生涯中，我从来没有真正看到它被有效地实现和维护过。

所以让我们尝试一些新的东西。年轻对象母亲模式。

# 年轻对象母亲模式

年轻对象母亲的概念很简单。它的工作方式是创建一个默认对象，该对象被设置为总是干净地通过验证，依靠 toBuilder 模式(一个保留其默认值的构建器模式)来更改使测试失败所需的字段。

```
public class PetProvider {
   public static Pet getDefaultDog(){
      return Pet.*builder*()
        .petType(PetType.*DOG*)
        .ageInMonths(1)
        .color("brown")
        .cuteness(10)
        .healthRating(10)
        .tail(*getDefaultTail*())
        .build();
   }
}public class TailProvider {
    public static Tail getDefaultTail(){
        return Tail
            .*builder*()
            .tailFluffiness(TailFluffiness.*SUPER_FLUFFY*)
            .tailLength(10)
            .build();
    }
}@Test
void validationFailsWhenPetNotCuteEnough() {
    // Immediately obvious that cuteness is effecting the result of the test
    var pet = *getDefaultDog*().toBuilder().cuteness(1).build();

    var petRequest = *getDemandingPetRequest*();

    NotCuteEnoughException notCuteEnoughException = *assertThrows*(NotCuteEnoughException.class,
        () -> petValidatorService.validatePet(pet, petRequest));
}

@Test
void validationFailsWhenPetIsTooOld() {
    var pet = *getDefaultDog*().toBuilder().ageInMonths(120).build();

    var petRequest = *getDemandingPetRequest*();

*assertThrows*(TooOldException.class,
        () -> petValidatorService.validatePet(pet, petRequest));
}
```

这种方法有三个优点。

首先，您只能在任何地方创建一个默认对象。
对你所有的测试。你所有的单元测试，集成测试，所有的合同测试，验收测试和任何其他类型的测试。如果我们现在添加一个大小验证，就我们的测试设置而言，我们需要做的就是修改一行代码。多好啊。

第二，也是更重要的一点是，哪个字段导致验证失败是显而易见的。这意味着它很容易理解和领会。

第三个优点是，通过创建更多的默认提供者方法，可以很容易地随时随地添加语义。但是，当采用这种方法时，我们再次没有明确指定哪个字段会导致测试失败。

```
public class TailProvider {
    public static Tail getDefaultTail(){
        return Tail
            .*builder*()
            .tailFluffiness(TailFluffiness.*SUPER_FLUFFY*)
            .tailLength(10)
            .build();
    }public static Tail getInvertedTail(){
        return Tail
            .*builder*()
            .tailFluffiness(TailFluffiness.*SUPER_FLUFFY*)
            .tailLength(-1)
            .build();
    }
}@Test
void validationFailsTailNotFluffyEnough() {
    var pet = *getDefaultDog*()
        .toBuilder()
        .tail(getInvertedTail())
        .build();

    var petRequest = *getDemandingPetRequest*();

*assertThrows*(InvalidTailLengthException.class,
        () -> petValidatorService.validatePet(pet, petRequest));

}
```

这种方法的主要缺点是需要在域对象上实现 toBuilder 模式(使用 Lombok)。

虽然为了适应测试而将生产代码与模式结合起来被认为是不好的做法，但我总是发现这在实践中很大程度上是不起作用的。

然而，为了降低这种风险，您可以使用 TestDataTemplate 模式。

# 测试数据模板模式

测试数据模板模式的概念与年轻对象母模式非常相似，唯一的区别是它创建了一个测试对象 POJO，该对象包含在原始 POJO 中创建的所有字段和一个生成器，该生成器不是在 build()方法中返回自身，而是返回一个原始 POJO 进行测试。

```
public class PetTestDataTemplate {
    public static PetTestDataTemplateBuilder builder() {
        return new PetTestDataTemplateBuilder();
    }

    public static class PetTestDataTemplateBuilder {
        private String name = "woofy";
        private Integer ageInMonths = 0;
        private Integer cuteness = 10;
        private Integer healthRating = 10;
        private String color = "brown";
        private PetType petType = PetType.*DOG*;
        private Tail tail = TailTestDataTemplate.*builder*().build();

        PetTestDataTemplateBuilder() {
        }

        public PetTestDataTemplateBuilder name(String name) {
            this.name = name;
            return this;
        }

        public PetTestDataTemplateBuilder ageInMonths(Integer ageInMonths) {
            this.ageInMonths = ageInMonths;
            return this;
        }

        public PetTestDataTemplateBuilder cuteness(Integer cuteness) {
            this.cuteness = cuteness;
            return this;
        }

        public PetTestDataTemplateBuilder invalidHealthRating(){
            this.healthRating = -1;
            return this;
        }

        public PetTestDataTemplateBuilder healthRating(Integer healthRating) {
            this.healthRating = healthRating;
            return this;
        }

        public PetTestDataTemplateBuilder color(String color) {
            this.color = color;
            return this;
        }

        public PetTestDataTemplateBuilder tail(Tail tail) {
            this.tail = tail;
            return this;
        }

        public PetTestDataTemplateBuilder petType(PetType petType) {
            this.petType = petType;
            return this;
        }

        public Pet build() {
            return Pet.*builder*()
                .name(name)
                .ageInMonths(ageInMonths)
                .cuteness(cuteness)
                .healthRating(healthRating)
                .color(color)
                .petType(petType)
                .tail(tail)
                .build();
        }
    }
}@Test
void validationFailsWhenPetHealthRatingIsBelow0() {
    var pet = PetTestDataTemplate.*builder*().invalidHealthRating().build();

    var petRequest = PetRequestDataTemplate.*builder*().build();

 *assertThrows*(InvalidHealthRatingException.class,
        () -> petValidatorService.validatePet(pet, petRequest));
}
```

正如我们在测试端看到的，这种模式几乎完全相同，并且在概念上极其相似。您只能看到影响测试结果的字段。好处是它不会把你的 POJOs 和任何创造模式联系起来，然而它有点*大*并且维护起来更痛苦，但是如果你是这样的话，我很高兴。

# 结论

总之，虽然您设置测试数据的方法可能看起来微不足道，但事实并非如此。在一个项目中，任何可以创建清晰性和减少 faf 的事情都不是浪费时间。

如果像我一样，你很懒，不介意将你的 POJOs 耦合到构建器和构建器创建模式，我会推荐 YOM(带有 lombok)。如果可以理解，你不想或者不能耦合你的 POJOs，任何创造性的模式，你可以维护测试对象，我会推荐 TDT 模式。