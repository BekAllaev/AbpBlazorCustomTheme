# Instruction on how to generate project with your custom theme and apply this theme to your Blazor project

This article is based upon this article - https://docs.abp.io/en/abp/latest/UI/Blazor/Theming?UI=Blazor#global-styles-script

Run this command in the root of our solution 
```
abp add-package Volo.Abp.AspNetCore.Components.WebAssembly.BasicTheme --with-source-code --add-to-solution-file
```

Create this directory tree `wwwroot` -> `libs` -> `abp` -> `css` and create file `theme.css` inside `css` folder. `theme.css` file will contain your custom styles

#### How to add project with themes to git stracking system

**Background:** When you created theme project using this CLI command - `abp add-package Volo.Abp.AspNetCore.Components.WebAssembly.BasicTheme --with-source-code --add-to-solution-file`
your project is ignored by git system by default so you can add this project to git system with following command:
```
git add --force packages
```

> "git add --force ***packages***", where ***packages*** is the name of directory where project with custom theme is generated

#### What to do when you added your custom styles?

Go to directory where your blazor project is and run `abp bundle` command

#### What is expected result?

Your `global.css` style should contain styles from `Volo.Abp.AspNetCore.Components.WebAssembly.BasicTheme`(this is the name of the razor class library that contain custom themes).
You can compare [styles](https://github.com/BekAllaev/AbpBlazorCustomTheme/blob/master/packages/Volo.Abp.AspNetCore.Components.WebAssembly.BasicTheme/wwwroot/libs/abp/css/theme.css) from `Volo.Abp.AspNetCore.Components.WebAssembly.BasicTheme` and [`global.css`](https://github.com/BekAllaev/AbpBlazorCustomTheme/blob/master/src/Acme.BookStore.Blazor/wwwroot/global.css#L32878)
from `Acme.BookStore.Blazor` and find out that styles from `Volo.Abp.AspNetCore.Components.WebAssembly.BasicTheme` have been copied to `global.css` (take a look to the line 32878).
Run solution and you will see that applied styles

#### What if you haven't get expected result?

- Make sure that path to the styles inside `BasicThemeBundleContributor` class in method `AddStyles` is right. You can take a look to right path in source code by pressing [there](https://github.com/BekAllaev/AbpBlazorCustomTheme/blob/master/packages/Volo.Abp.AspNetCore.Components.WebAssembly.BasicTheme/BasicThemeBundleContributor.cs#L14C17-L14C17)
- Make sure that your file with styles have **"build action"** = *"content"* and **"copy to output directory"** = *"copy if newer"*

#### What you should do if you want to move the theme to another solution

1). Move `Volo.Abp.AspNetCore.Components.WebAssembly.BasicTheme` to the solution
2). Add reference to this project from your blazor project
3). Add `DependsOn` so your `(ProjectName)BlazorModule` class will be like this:
```
[DependsOn(
typeof(........),
typeof(........),
typeof(........),
typeof(AbpAspNetCoreComponentsWebAssemblyBasicThemeModule))] <---This is the name of the module where your custom theme is
public class (ProjectName)BlazorModule : AbpModule
{
}
```
4). Run `abp bundle` in the directory where your blazor project is

If you have some problem please read "**What if you haven't get expected result?**"
