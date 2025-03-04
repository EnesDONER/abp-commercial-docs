# FAQ System

The CMS kit provides a **FAQ** system to allow users to create, edit and delete FAQ's. Here is a screenshot of the FAQ widget:

![cmskit-module-faq-widget](../../images/cmskit-module-faq-widget.png)

## Enabling the FAQ System

By default, CMS Kit features are disabled. Therefore, you need to enable the features you want, before starting to use it. You can use the [Global Feature](https://docs.abp.io/en/abp/latest/Global-Features) system to enable/disable CMS Kit features on development time. Alternatively, you can use the ABP Framework's [Feature System](https://docs.abp.io/en/abp/latest/Features) to disable a CMS Kit feature on runtime.

> Check the ["How to Install" section of the CMS Kit Module documentation](index.md#how-to-install) to see how to enable/disable CMS Kit features on development time.

## User Interface

### Menu Items

CMS Kit module admin side adds the following items to the main menu, under the **CMS** menu item:

**FAQ's**: FAQ management page.

`CmsKitProAdminMenus` class has the constants for the menu item names.

### Pages

You can list, create, update and delete FAQ's on the admin side of your solution.

![faq-page](../../images/cmskit-module-faq-page.png)
![faq-edit-page](../../images/cmskit-module-faq-edit-page.png)
![faq-edit-question-page](../../images/cmskit-module-faq-edit-question-page.png)

## Faq Widget

The FAQ system provides a FAQ [widget](https://docs.abp.io/en/abp/latest/UI/AspNetCore/Widgets) for users to display FAQ's. You can place the widget on a page like below:

```csharp
@await Component.InvokeAsync(
    typeof(FaqViewComponent),
    new
    {
        groupName = "group1",
        name = ""
    })
```

## Options

Before using the FAQ system, you need to define groups. You can use `FaqOptions`. `FaqOptions` can be configured at the domain layer, in the `ConfigureServices` method of your [module](https://docs.abp.io/en/abp/latest/Module-Development-Basics).
  
```csharp
  Configure<FaqOptions>(options =>
    {
        options.AddGroups("group1");
        options.AddGroups("group2");
        options.AddGroups("group3");
    });
```

`FaqOptions` properties:

- `GroupName`: List of defined groups in the FAQ system. The `options.AddGroups` method is a shortcut to add a new group to this list.

## Internals

### Domain Layer

#### Aggregates

This module follows the [Entity Best Practices & Conventions](https://docs.abp.io/en/abp/latest/Best-Practices/Entities) guide.

##### FAQ

A FAQ represents a generated FAQ with its questions: 

- `FaqSection` (aggregate root): Represents a FAQ by including the options in the system.
- `FaqQuestion` (entity): Represents the defined FAQ questions related to the FAQ in the system.

#### Repositories

This module follows the guidelines of [Repository Best Practices & Conventions](https://docs.abp.io/en/abp/latest/Best-Practices/Repositories).

The following special repositories are defined for these features:

- `IFaqSectionRepository`
- `IFaqQuestionRepository`


#### Domain services

This module follows the [Domain Services Best Practices & Conventions](https://docs.abp.io/en/abp/latest/Best-Practices/Domain-Services) guide.


### Application layer

#### Application services

- `FaqAdminAppService` (implements `IFaqAdminAppService`): Implements the use cases of FAQ management for admin side.
- `FaqPublicAppService` (implements `IFaqPublicAppService`): Implements the use cases of FAQ's for public websites.

### Database providers

#### Common

##### Table / collection prefix & schema

All tables/collections use the `Cms` prefix by default. Set static properties on the `CmsKitDbProperties` class if you need to change the table prefix or set a schema name (if supported by your database provider).

##### Connection string

This module uses `CmsKit` for the connection string name. If you don't define a connection string with this name, it fallbacks to the `Default` connection string.

See the [connection strings](https://docs.abp.io/en/abp/latest/Connection-Strings) documentation for details.

#### Entity Framework Core

##### Tables

- CmsFaqSections
  - CmsFaqQuestions

#### MongoDB

##### Collections

- **CmsFaqSections**

## Entity Extensions

Check the ["Entity Extensions" section of the CMS Kit Module documentation](index.md#entity-extensions) to see how to extend entities of the FAQ Feature of the CMS Kit Pro module.