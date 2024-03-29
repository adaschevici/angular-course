---
path: "/angular-components-ii"
title: "Angular Components (part II)"
order: 6
---

<iframe src="https://docs.google.com/presentation/d/1LsQ5ePWNPFR6nvXh4VhqYpUPMUkaQC0wMwEo0Pyl1no/embed?start=false&loop=false&delayms=30000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

## Angular Components (part II)

To start on this chapter you can either work on your branch or
`git checkout 01-angular-components`

⚠️ If you get an error like:

```shell
Please commit your changes or stash them before you switch branches.
Aborting
```

run the following commands that will store your changes on the git stash

`git stash --include-untracked`

`git checkout 01-angular-components`

## Let's start

Components provide us with a way to build reusable UI interfaces. Most modern frameworks and libraries have
this approach to building UIs.
In the previous chapter we started building some basic Angular features using the `@angular/cli`. This is useful
for training the reflex of reaching out to the tools that the framework provides. As they say _"Always use the right tool for the job"_

In this chapter we will build a search component that will allow us to filter the books array based on the
title.

### 1. Create a new component

Using the `@angular/cli` create the `search-box` component

`npx ng g c search-box`

This will generate an angular component for our search. Given that we added material in our previous
lecture we will be using angular material throughout for building our components

```bash
▾ [ ] src/
  ▾ [ ] app/
    ▾ [ ] search-box/
        [ ] search-box.component.html
        [ ] search-box.component.scss
        [ ] search-box.component.ts
```

The content of the component should be able to manage input from the search box.

```javascript
...
import { Search } from '../shared/search.model'; // we implemented a Search type for the search box

@Component({
  selector: 'app-search-box',
  templateUrl: './search-box.component.html',
  styleUrls: ['./search-box.component.scss']
})
export class SearchBoxComponent implements OnInit {

  searchTerm: string = '';
  @Output() searchFired = new EventEmitter<Search>();

  constructor() { }

  ngOnInit(): void {
  }

  onChange(value: string) {
    this.searchTerm = value;
  }

  onSearch() {
    this.searchFired.emit({
      searchTerm: this.searchTerm
    })
  }
}
```

We should have a template for it that makes use of the Angular Material themed components
In order to be able to use material components in angular the material modules need to be imported in the
main app module.

```javascript
...
import { MatInputModule } from '@angular/material/input';
import { MatButtonModule } from '@angular/material/button';
...
@NgModule({
  declarations: [
    AppComponent,
    SearchBoxComponent,
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    BrowserAnimationsModule,
    MatInputModule,
    MatButtonModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```

```html
<div class="search-box">
  <mat-form-field class="example-full-width" appearance="outline">
    <mat-label>Search book by title</mat-label>
    <input matInput value="" (input)="onChange($event.target.value)" />
  </mat-form-field>
  <button
    mat-raised-button
    color="primary"
    class="submit-btn"
    (click)="onSearch()"
  >
    Search
  </button>
</div>
```

The `app-root` component will become this:

```javascript
@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.scss"]
})
export class AppComponent {
  title = "ng-goodreads";
  searchTitle: string = "";
  books: Array<Book> = [<extracted list of books from server/books.json>];

  doSearch(search: Search) {
    this.searchTitle = search.searchTerm;
  }
}
```

### Individual Exercises

1. Add `ngStyle` or `ngClass` to the search box to align the button and make it bigger
2. Add a typescript model for book objects under `src/app/shared/book.model.ts` and use it in the components
3. Make the search perform be case insensitive
