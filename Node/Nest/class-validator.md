### 字符串类型的判断

字符串"1"，可以符合条件。数字1却不行。

字符串"1"，可以符合条件。数字1却不行。

注意，错误的message要这么传入
```
@IsNumberString({},{message: '....'})
```

#### IsBooleanString
有了上边的经验，"true",'false'可以通过判断

#### IsDateString
符合日期类型的字符串

### 字符串类型

#### Contains
传入的字符串包含制定的字符串么

@Contains('test'),包含test通过。不包含，抛出异常。

####  message为第二个入参数。

####  NotContains
与上相反

####  IsUppercase
是否都是大写

#### IsLowercase
小写

#### IsMobilePhone
是否是手机号
你需要这样使用 @IsMobilePhone('zh-cn'),看懂了吗，需要传一个locate。


#### @Length(min: number, max?: number)
检验字符串长度的。注意一下跟int的不同

#### MaxLength &&& MinLength
这个很简单，直接
```
@MaxLength(12)
```

Matches
使用时，第一个参数是一个正则，符合该正则通过。

```
@Matches(/foo/i)
```

#### IsJson
输入的字符串是个JSON格式的


### 类型判断的装饰器
#### IsString
是string类型

#### IsDate
必须是Date类型了

#### IsNumber
Number类型就可以

#### IsInt
整数通过

#### IsArray
必须是数组

#### IsEnum(entity: object)
#### IsObject
判断是否是一个对象

#### IsNotEmptyObject()
不是空对象

### 常用校验器

#### IsDefined
不能等于null或者undefined

空字符串可以的

#### IsOptional 常用
可选的。如果输入的等于null或者undefined，忽略其它的校验装饰器

#### Equals
等于某个确定的值
```
@Equals(5)
```

#### NotEquals
不等于某个值

#### IsEmpty
检查这个值等于'',null,undefined。

#### IsNotEmpty
不能是空的，也就是不能是'',undefined,null

#### IsIn
检查值是否是数组中的某个值
```
@IsIn(['1','2'])
```
输入的值必须是1,2中的一个值。

####  IsNotIn
校验的值不能是给定的数组中的元素
```
@IsIn(['1','2'])
```
### 错误信息
除了内定的错误信息，可以自己设置message属性。
```
export class Post {
  @MinLength(10, {
    message: 'Title is too short',
  })
  @MaxLength(50, {
    message: 'Title is too long',
  })
  title: string;
}
```
像@IsEmpty这种，message设置到第一个参数
```
@IsEmpty({message: '....'})

```


### 补充： 校验数组
如果需要校验数组中的每一项。需要传入一个 each: true

比如期望输入的一个数组，每一个元素应该是一个小于10的number，
```
@IsNumber({},{each: true,})
@Max(20,{each: true})
```
不舒服的一点就是每个装饰器都要传入each: true


### 自定义装饰器

https://github.com/typestack/class-validator#custom-validation-decorators


Create a decorator itself:
```
import { registerDecorator, ValidationOptions, ValidationArguments } from 'class-validator';

export function IsLongerThan(property: string, validationOptions?: ValidationOptions) {
  return function (object: Object, propertyName: string) {
    registerDecorator({
      name: 'isLongerThan',
      target: object.constructor,
      propertyName: propertyName,
      constraints: [property],
      options: validationOptions,
      validator: {
        validate(value: any, args: ValidationArguments) {
          const [relatedPropertyName] = args.constraints;
          const relatedValue = (args.object as any)[relatedPropertyName];
          return typeof value === 'string' && typeof relatedValue === 'string' && value.length > relatedValue.length; // you can return a Promise<boolean> here as well, if you want to make async validation
        },
      },
    });
  };
}
```
Put it to use:

```
import { IsLongerThan } from './IsLongerThan';

export class Post {
  title: string;

  @IsLongerThan('title', {
    /* you can also use additional validation options, like "groups" in your custom validation decorators. "each" is not supported */
    message: 'Text must be longer than the title',
  })
  text: string;
}
```

