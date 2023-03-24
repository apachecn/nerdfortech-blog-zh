# 角形材料作为可重复使用组件的基础

> 原文：<https://medium.com/nerd-for-tech/angular-material-as-a-base-for-reusable-components-163b7e3d928e?source=collection_archive---------3----------------------->

![](img/3ecbc6455f09aff5e4bc7c48fa4ba729.png)

大家好！让我们来谈谈 Angular Material，这是一个由 google 创建的超级酷的 UI 库，我们 Angular 开发人员喜欢在我们的项目中使用它。但是…如果我们用错了呢？我明白了，这个库使用的一些组件非常简单，只需要复制和粘贴就可以让它们工作，对于更复杂的元素也是一样，比如自动完成…对吗？。

让我们看一些例子:

[https://material.angular.io/components/input/overview](https://material.angular.io/components/input/overview)

**用于角度材料输入的 HTML】**

```
<input matInput placeholder="Favorite food" value="Sushi">
```

很简单！我们仍然可以在某种程度上改变它，这样我们就可以以一种反应的形式使用它:

**TS:**

```
export class OurComponent implements OnInit {
  form: FormGroup;
  constructor(fb: FormBuilder){} ngOnInit() {

     this.form = this.fb.group({
        sushi: [""],
     });

   }
}
```

**HTML:**

```
<form [formGroup]="form"> <mat-form-field> <mat-placeholder>Your Sushi Input</mat-placeholder> <textarea matInput formControlName="sushi"></textarea>

   </mat-form-field>    
 </form>
```

超级简单！，但是…稍微复杂一点的东西会怎么样？

[https://material . angular . io/components/autocomplete/overview](https://material.angular.io/components/autocomplete/overview)

**带过滤器的角度材料自动完成的 HTML**

```
<form class="example-form">

   <mat-form-field class="example-full-width">

   <input type="text" placeholder="Pick one" aria-label="Number" matInput [formControl]="myControl" [matAutocomplete]="auto">

   <mat-autocomplete #auto="matAutocomplete">
      <mat-option *ngFor="let option of filteredOptions | async" [value]="option">
        {{option}}
      </mat-option>
   </mat-autocomplete> </mat-form-field>
</form>
```

**TS:**

```
export class AutocompleteFilterExample implements OnInit {
  myControl = new FormControl();
  options: string[] = ['One', 'Two', 'Three'];
  filteredOptions: Observable<string[]>; ngOnInit() {
    this.filteredOptions = this.myControl.valueChanges
      .pipe(
        startWith(''),
        map(value => this._filter(value))
      );
  } private _filter(value: string): string[] {
    const filterValue = value.toLowerCase(); return this.options.filter(option => {
        option.toLowerCase().includes(filterValue));
    }
  }
}
```

还是很容易实现的…但是如果我们只做一次…..让我们看看如果我们有不止一个会发生什么:

**HTML:**

```
<form class="example-form">

   <mat-form-field class="example-full-width">

   <input type="text" placeholder="Pick one" aria-label="Number" matInput [formControl]="myControl" [matAutocomplete]="auto">

   <mat-autocomplete #auto="matAutocomplete">
      <mat-option *ngFor="let option of filteredOptions | async" [value]="option">
        {{option}}
      </mat-option>
   </mat-autocomplete> <mat-form-field class="example-full-width">

   <input type="text" placeholder="Pick another one" aria-label="Number" matInput [formControl]="myControl2" [matAutocomplete2]="auto2">

   <mat-autocomplete #auto="matAutocomplete2">
      <mat-option *ngFor="let option of filteredOptions2 | async" [value]="option">
        {{option}}
      </mat-option>
   </mat-autocomplete> </mat-form-field>
</form>
```

**TS:**

```
export class AutocompleteFilterExample implements OnInit {
  myControl = new FormControl();
  options: string[] = ['One', 'Two', 'Three'];
  options2: string[] = ['Four', 'Five', 'Six'];
  filteredOptions: Observable<string[]>;
  filteredOptions2: Observable<string[]>; ngOnInit() {
    this.filteredOptions = this.myControl.valueChanges
      .pipe(
        startWith(''),
        map(value => this._filter(value))
      );
    this.filteredOptions2 = this.myControl2.valueChanges
      .pipe(
        startWith(''),
        map(value => this._filter(value))
      );
  } private _filter(value: string): string[] {
    const filterValue = value.toLowerCase(); return this.options.filter(option => {
        option.toLowerCase().includes(filterValue));
    }
  }
}
```

它开始变得有点臃肿了，对吗？，现在想象一个使用 10 个自动完成组件的表单…..我们需要创建 10 个过滤选项，在单个组件中更改它们各自的值…我们可以做得更好！！！

所以让我们试着换个角度思考:

首先，让我们基于角形材料创建新的自动完成组件，同时我们还可以增加其功能:

**HTML:**

```
<ng-container *ngIf="_options.length > 0; else noOptionsTemplate">
  <mat-form-field class="full-width">
    <input type="text" matInput [placeholder]="completePlaceholder" [formControl]="control" [matAutocomplete]="auto" />
    <mat-autocomplete
      #auto="matAutocomplete"
      [displayWith]="getLabelName.bind(this)"
      (optionSelected)="selectionChange ? selectionChange($event) : null"
    >
      <mat-option *ngFor="let option of filteredOptions | async" [value]="option.value" [title]="option.label">
        {{ option.label }}
      </mat-option>
    </mat-autocomplete>
    <mat-error>
      <app-input-error-messages [control]="control"></app-input-error-messages>
    </mat-error>
  </mat-form-field>
</ng-container>
<ng-template #noOptionsTemplate>
  <mat-form-field class="full-width">
    <input type="text" matInput [placeholder]="completePlaceholder" [formControl]="dummyControl" />
  </mat-form-field>
</ng-template>
```

**TS:**

```
@Component({
  selector: 'app-auto-complete',
  templateUrl: './autocomplete.component.html',
  styles: ['.full-width { width: 100% }']
})
export class AutoCompleteComponent implements OnInit {
  @Input() control = new FormControl();
  @Input() set options(value: any[]) {
    this._options = [...(value ? value : [])];
    this.disableControl(this._options.length === 0);
    this.checkValidations();
  }
  @Input() placeholder: string;
  @Input() selectionChange: Function;
  @Input() set disabled(value: boolean) {
    this.disableControl(value);
  }
  @Input() set required(value: boolean) {
    this._required = value;
    this.checkValidations();
    this.completePlaceholder = `${this.placeholder} ${this._required ? '*' : ''}`;
  }
 private lastValue: string;
  dummyControl = new FormControl('');
  completePlaceholder = 'Elija una opción';
  _options: SelectOption[];
  _required: boolean;
  filteredOptions: Observable<SelectOption[]>; constructor(private validationService: ValidationService) {
    this.dummyControl.disable();
  } ngOnInit() {
    this.filteredOptions = this.control.valueChanges.pipe(
      startWith(''),
      debounceTime(300),
      map(value => this._filter(value))
    );
    this.completePlaceholder = `${this.placeholder} ${this._required ? '*' : ''}`;
  } private checkControlValue() {
    return this.control && this.control.value ? this.control.value : '';
  } private checkValidations() {
    const validators = !!this._required
      ? [this.validationService.requireMatch(this._options)]
      : [this.validationService.requireMatchWithEmpty(this._options)];
    this.control.setValidators(validators);
    this.control.updateValueAndValidity();
  } private _filter(value: string): SelectOption[] {
    const filterValue = value ? value.toString().toLowerCase() : '';
    if (this.lastValue && filterValue !== this.lastValue && filterValue === '' && this.selectionChange)
      this.selectionChange({ option: { value: this.checkControlValue() } });
    this.lastValue = filterValue;
    return this._options ? this._options.filter(option => option.label.toLowerCase().includes(filterValue)) : this._options;
  } private disableControl(value: boolean) {
    if (value) {
      this.control.disable({ emitEvent: false });
    } else {
      this.control.enable({ emitEvent: false });
    }
  } getLabelName(value: string) {
    const result = this._options ? this._options.find(option => option.value === value) : null;
    return result ? result.label : undefined;
  }
}
```

自定义验证服务，用于验证用户的输入是否与某些选项匹配:

```
import { Injectable } from "@angular/core";
import { AbstractControl } from "@angular/forms";

@Injectable()
   export class ValidationService {
      getValidatorErrorMessage(validatorName: string, validatorValue?: any) {
	    const config = {
	      required: "This field is required",
	      requireMatch: "Please, enter a valid option"
	    };  return config[validatorName];
	 }  requireMatch(options) {
	    return (control: AbstractControl) => {
	      const selection: any =
	        options && options.find(o => o.value === control.value)
	          ? null
	          : { requireMatch: true };
	      return selection;
	    };
	  }  requireMatchWithEmpty(options) {
	    return (control: AbstractControl) => {
	      const selection: any =
	        (options && options.find(o => o.value === control.value)) ||
	        !control.value
	          ? null
	          : { requireMatch: true };
	      return selection;
	    };
	  }
}
```

我们将如何使用它？

**HTML:**

```
<form class="example-form"> <app-auto-complete
     [control]="form.get('myControl')"
     [options]="options1"
     placeholder="Pick one"
     [required]="true"
   >
   </app-auto-complete>

   <app-auto-complete
     [control]="form.get('myControl2')"
     [options]="options1"
     placeholder="Pick another one"
     [required]="true"
   >
   </app-auto-complete></form>
```

**TS:**

```
export class AutocompleteFilterExample implements OnInit {
  myControl = new FormControl();
  myControl2 = new FormControl(); options: string[] = ['One', 'Two', 'Three'];
  options2: string[] = ['Four', 'Five', 'Six'];  ngOnInit() {}
}
```

更容易阅读对吗？

让我们检查一下创建这个组件的改进，这些也适用于您以这种方式创建的其他组件:

**1-** 我们从有角材料的构件逻辑中，划分出我们构件的逻辑。

**2-** 更容易阅读代码。

更容易测试，因为它是一个可重用的组件，一旦通过测试，它将在整个应用程序中同样工作。

**4-** 相同的风格，因为它是一个可重用的组件，所以整个应用程序都将具有相同的风格，避免错误。

**5-** 我们可以增加角度材质的组件功能，在这种情况下，我们检查选项，因此当它为空时，输入将被禁用，我们还检查用户的输入是否与其中一个选项匹配。

> 原来如此！我希望这对你将来的代码和项目有所帮助，感谢阅读！
> 
> 我也想邀请你来我的前端社区，因为[不和](https://discord.gg/KEavKkDc5Y)！在那里，我通过我的 [YouTube](https://www.youtube.com/watch?v=2jfIfeY4lrQ&t=1006s&ab_channel=GentlemanProgramming) 频道进行指导和授课。
> 大家编码快乐！！