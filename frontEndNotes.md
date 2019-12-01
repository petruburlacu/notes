## Best Practices

Single responsibility principle
https://code-maze.com/angular-best-practices/

Pollyfills | IE: https://blog.angularindepth.com/angular-and-internet-explorer-5e59bb6fb4e9

* [Angular](https://angular.io/) -- the latest version of angular, packed with new features and performant components
* [ng-zorro-antd](https://ng.ant.design/docs/getting-started/en) -- Ant Design library for angular, quality components with the focus on performance
* [bootstrap](https://ng-bootstrap.github.io/#/getting-started) -- open source toolkit for developing HTML, CSS and JS applications
* [highcharts](https://www.highcharts.com/docs/) -- highly configurable charts
* [ag-grid](https://www.ag-grid.com/angular-more-details/) -- feature rich library for tables, focused on performance and compatibility
* [font-awesome](https://fontawesome.com/how-to-use/on-the-web/referencing-icons/basic-use) -- clean and accessible library for icons
* [pacejs](https://github.hubspot.com/pace/docs/welcome/) -- automatic and fast page load progress bar

### Using Interfaces
If we want to create a contract for our class we should always use interfaces. By using them we can force the class to implement functions and properties declared inside the interface.

Using interfaces is a perfect way of describing our object literals. If our object is of an interface type, it is obligated to implement all of the interface’s properties.

```typescript
export interface User {
    name: string;
    age: number;
    address: string;
} 
```
>The TypeScript will show an error if an object doesn’t contain all of the interface’s properties, and light up intellisense for us while populating that object.

We can specify optional properties, by using the question mark (?) inside an interface as well. We don’t need to populate those properties inside an object:

```typescript
additionalData?: string;
```


### Data Manipulation | The Immutable Way

By modifying reference types immutably, we are preserving the original objects and arrays and modifying only their copies.

```typescript
// Copy object without reference
newObject = JSON.parse(JSON.stringify(oldObject));

// Deep compare two complex objects
JSON.stringify(objectOne) !== JSON.stringify(objectTwo)

// Replace .push() with this
let list: type[] = [];
list = [...list, entry];


// Deep copy the values of all enumerable own properties from one or more source objects to a target object
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

### Safe Navigation Operator (?) in HTML Template
To be on the safe side we should always use the safe navigation operator while accessing a property from an object in a component’s template. If the object is null and we try to access a property, we are going to get an exception. But if we use the save navigation (?) operator, the template will ignore the null value and will access the property once the object is not the null anymore.

```typescript
<div>
  {{user?.name}}
</div>
```

### HTTP Request

Requests should be handled in the service layer.

```typescript
import { HttpClient } from '@angular/common/http';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import 'rxjs/add/operator/map';
import { Observable } from 'rxjs';
```

Post request example:

```typescript
postRequestMethod(request): Observable<ResponseModel> {
    return this.http.post("/request-path", request)
    .map((response: ResponseModel) => response).catch((err: any) => Observable.throw(err.error || 'error'));
}
```

Delete request example:

>Sending HTTP requests isn’t always that simple. Many times we have to set up the headers of the request.

```typescript
deleteRequestMethod(request): Observable<ResponseModel> {
    const requestOptions = {
      // Request configuration
      headers: new HttpHeaders({'Content-Type': 'application/json'}),
      body: request
    };
    return this.http.delete("/request-path", requestOptions)
    .map((response: ResponseModel) => response).catch((err: any) => Observable.throw(err.error || 'error'));
}
```

HttpClient, Services and Environment Files: https://code-maze.com/net-core-web-development-part9/#angularRepository

### Handling Observables in Application's Life Cycle
```typescript
import { Subject } from 'rxjs';

ngUnsubscribe = new Subject();

ngOnDestroy() {
    this.ngUnsubscribe.next();
    this.ngUnsubscribe.complete();
  }

methodExample(): void {
    this.service.httpRequestMethod(object).pipe(takeUntil(this.ngUnsubscribe))
      .subscribe((response: ResponseModel) => {
        // Custom logic
      }, error => {
        console.log(error);
      }, () => {
        // Final logic
      });
}
```

### Angular project structure

The best place to educate yourself about angular applications is their own [guidelines](https://angular.io/guide/styleguide).

```
|-- app
    |-- pages
        |-- [+] Home, Login, Dashboard
    |-- core
       |-- [+] authentication
       |-- [+] configs
       |-- [+] guards
       |-- [+] http
       |-- [+] interceptors
       |-- [+] mocks
       |-- [+] services
       |-- core.module.ts
       |-- logger.service.ts
    |
    |-- shared
        |-- [+] reusable components
            |-- [+] header, table, etc.
        |-- [+] directives
        |-- [+] pipes
        |-- [+] models
    |
    |-- app-routing.module.ts
    |-- app.module.ts
|-- assets
    |-- [+] images
    |-- [+] styles
    |-- plugins
        |-- [+] bootstrap
        |-- [+] font-awesome
        |-- [+] pace
```

ViewChild elements

```typescript
@ViewChild('htmlElement') private htmlElement: TemplateRef<any>;
```

### Ant Design

Fixing collapsable menu initialization

```typescript
ngOnInit() {
    this.service.postAuthentication().subscribe(() => {
      this.pupulateMenus();
    }, (error) => {
      console.log(error);
      });
    // Fixes a known bug for the initialised collapsed menu | NgZorro
    setTimeout(() => {
        this.isCollapsed = true;
    });
  }
```

Custom table component height

```typescript
setHeight(rowSize: number): string {
    let screenPercentage = 0.55;
    if (this.isSummary) {
      screenPercentage = 0.50;
    }
    const height = rowSize * tableData.length + 10;
    const windowHeight = window.screen.height * screenPercentage;
    return height > windowHeight ? '56vh' : height + 'px';
  }
```

Dark mode
>NOTE: Use cookies to save user preferences. Util method to Create / Read cookies.

```scss
.dark-theme {
    @import '../node_modules/ng-zorro-antd/ng-zorro-antd.less';

    @ant-prefix: ant;

    // -------- Colors -----------
    @primary-color: #ff4713;
    @info-color: #ff4713;
    @success-color: @green-6;
    @processing-color: #ff4713;
    @error-color: @red-6;
    @highlight-color: #ffce00;
    @warning-color: @gold-6;
    @normal-color: #d9d9d9;

    ...
}
```

Handle NgZorro & Papaparse imports

```typescript
beforeUpload = (file: File) => {
    this.fileList = [];
    this.fileList = [...this.fileList, file];
    return new Observable((observer: Observer<boolean>) => {
      const reader: FileReader = new FileReader();
      reader.readAsText(file);
      reader.onloadend = (e) => {
        const result = reader.result;
        const parsed = parse(result, Util.parseConfiguration());
        // Custom logic
      };
      reader.onerror = (e) => {
        console.log(e);
      };
      observer.complete();
    });
}
```

Ts-XLSX

```typescript
beforeUpload = (file: File) => {
    if (this.fileList.length !== 0) this.fileList = [];
    this.fileList = [...this.fileList, file];
    return new Observable((observer: Observer<boolean>) => {
        const reader: FileReader = new FileReader();
        reader.readAsText(file);
        reader.onloadend = (e) => {
            const data = reader.result;
            const table = XLSX.read(data, {type: 'binary'});
            const sheet = table.Sheets[table.SheetNames[0]];
            const workbook = XLSX.utils.sheet_to_json(sheet, {range: 4});
            console.log(workbook);
            const range = XLSX.utils.decode_range(sheet['!ref']);
            // s = start | e = end | r = row | c = column
            for (let rowNum = range.s.r; rowNum <= range.e.r; rowNum++) {
                // Get rows bigger than 3
                const secondCell = sheet[XLSX.utils.encode_cell({r: rowNum, c: 1})];
            }
        };
        reader.onerror = (e) => {
        console.log(e);
        };
        // reader.readAsBinaryString(file);
        observer.complete();
    });
}
```

Papaparse export

```typescript
exportCSV() {
    const data = unparse(customData, Util.unparseConfiguration());
    const fileName = [];
    fileName.push('fileNameExample.csv', '.csv');
    const exportFilename = fileName.join('');
    if (!data) return;
    const csvData = new Blob([data], { type: 'text/csv;charset=utf-8;' });
    // IE11 & Edge
    if (navigator.msSaveBlob) {
      navigator.msSaveBlob(csvData, exportFilename);
    } else {
      // In FF link must be added to DOM to be clicked
      const link = document.createElement('a');
      link.href = window.URL.createObjectURL(csvData);
      link.setAttribute('download', exportFilename);
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }
  }
```

Regex Validation

```typescript
validate(event): boolean { return ((/["]/).test(event)) ? false : true; }
```

Navigation

```typescript
redirectMethod() {
    return this.router.navigate(['/path']);
}

navigate() {
    return window.location.href = '/url';
}
```

### Jest

https://itnext.io/how-to-use-jest-in-angular-aka-make-unit-testing-great-again-e4be2d2e92d1

```json
  "jest": {
    "preset": "jest-preset-angular",
    "setupFilesAfterEnv": [
      "<rootDir>/setup-jest.ts"
    ],
    "testResultsProcessor": "jest-sonar-reporter",
    "testURL": "http://localhost/",
    "verbose": true
  }
```

### SCSS
```json
"defaultProject": "app-name",
  "schematics": {
    "@schematics/angular:component": {
      "styleext": "scss"
    }
  }
```

### Aliases for imports
Add the following code into tsconfig.json file to make your imports short and well organized:

```typescript
{
  "compileOnSave": false,
  "compilerOptions": {
    removed for brevity,
    "paths": {
      "@app/*": ["app/*"],
      "@cmp/*": ["components/*"]
    }
  }
}
```

### Pipes
```typescript
@Pipe({ name: 'safeUrl' })
export class SafeUrlPipe implements PipeTransform {
  constructor(private sanitizer: DomSanitizer) {}
  transform(url: string) {
    return this.sanitizer.bypassSecurityTrustResourceUrl(url);
  }
}
```

```
[src]="legacyURL | safeUrl"
```
https://scotch.io/tutorials/reference-angular-imports-absolutely-for-easier-development

### SVG Isses

Display ratio is a very common issue with SVG's in Internet Explorer.
Workaround:
	- Make sure that your CSS is compatible with IE
	- Open the used SVG with your favourite editor and add this property to your SVG: `preserveAspectRatio="xMaxYMid meet"`

This should preserve the aspect ratio of the SVG when using the CSS properties.
*NOTE: Make sure to have the `xml:space` equal to `"preserve"`

### Mime Type issues for Angular 8+
- on builds for production: change the .js file types from ''' type="module" ''' to ''' type="text/javascript" ''' in the **index.hml** file.


## Useful Links

Light Screen Recorder (GIFs): https://github.com/ShareX/ShareX

**Javascript 101**: https://github.com/leonardomso/33-js-concepts

Angular Education: https://github.com/timjacobi/angular-education

Angular Fire Lite: https://github.com/hamedbaatour/angularfire-lite

Security Guide: https://github.com/FallibleInc/security-guide-for-developers

**OPEN SOURCE APIs**: https://www.programmableweb.com/category/open-source/api

CSS Cheatsheet: https://30-seconds.github.io/30-seconds-of-css/

Front End Performance Checklist: https://github.com/thedaviddias/Front-End-Performance-Checklist

Interactive Card Input: https://jessepollak.github.io/card/

Remove Unused CSS: https://github.com/uncss/uncss

**Angular PIPELINES**: https://github.com/danrevah/ngx-pipes#diff

ImmutableJs Library: https://github.com/immutable-js/immutable-js

Testing Angular Applications Book: https://www.manning.com/books/testing-angular-applications

Conference Talks: https://github.com/emmawedekind/badass-conference-talks

JavaScript Algorithms: https://github.com/mgechev/javascript-algorithms

Project Based Learning: https://github.com/tuvtran/project-based-learning#java

History Session JavaScript: https://github.com/ReactTraining/history

Foundation Premade HTML Templates: https://foundation.zurb.com/templates.html

Background Patters: https://github.com/atlemo/SubtlePatterns
