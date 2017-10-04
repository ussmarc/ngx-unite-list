# ngx-unitelist

This librabry is for Angular (2+) projects to build a list from the data passed and provide pagination and filters and their callbacks after passing a configuration properly

## Installing

```
npm install @appcarvers/ngx-unitelist --save
```
To use pagination and filters of unitelist, you will also need to install few dependencies as listed below -
```
npm install ngx-bootstrap --save
npm install ng2-select --save
```


## Implementation

Simply first import the module in your app.module.ts as shown below

```
import { UniteModule } from '@appcarvers/ngx-unitelist/unite.module';

imports : [UniteModule]

```

Now, pass proper configuration so to render pagination, filters, table-view. You will also get various callback like pageChanged and filterChanged so that you can update the data based on changes.

### Code in app.component.html

user.component.html
```
    <ngx-unite-list
        [data]="usersData"
        [tableHeaders] = 'usersHeaders'
        [totalPages] = 'userTotalPages'
        [currentPage] = 'userCurrentPage'
        [filters] = 'userFilters'
        [searchBox] = 'true'
        (pageChanged)='checkPageChanged($event)'
        table-class = 'table-bordered table'
    > </ngx-unite-list>
```

user.component.ts
```
    import { Component, OnInit } from '@angular/core';
    import { FakedataService } from '../fakedata.service';

    @Component({
    selector: 'app-userlist',
    templateUrl: './userlist.component.html',
    styleUrls: ['./userlist.component.css'],
    providers : [FakedataService]
    })
    export class UserlistComponent implements OnInit {

    usersData;
    usersHeaders;
    userTotalPages;
    userCurrentPage;
    userFilters;

    constructor(private _fService : FakedataService) { }

    ngOnInit() {
        this.usersHeaders = [
                                {label : "Id", identifier : ['id']},
                                {label : "Name", identifierCombo : [['first_name'],['last_name']]},
                                {label : "Avatar", identifier : ['avatar'], displayType : 'image'}
                            ];

        this.userFilters =  [
                             {
                               label     : 'Filter 1',
                               filterId  : 'filter-1',
                               options   : [{id: 'a', text: 'Alpha'},{id: 'b', text: 'Beta'},{id: 'c', text: 'Gamma'},]
                             },
                             {
                               label : 'Filter 2',
                               filterId : 'filter-2',
                               options : [{id: 'a', text: 'xyz'},{id: 'b', text: 'abc'},{id: 'c', text: 'syz'},]
                             } 
                            ];

        this.loadUsers();
    }

    loadUsers(pageNo? : number){
        this._fService.getUsers(pageNo).subscribe(usersData => {
        this.usersData      = usersData['data'];
        this.userTotalPages = usersData['total_pages'];
        this.userCurrentPage = usersData['page'];
        console.log(usersData);
        });
    }

    checkPageChanged(e){
        if(typeof e.newPage === 'number')
        {
        this.loadUsers(e.newPage);
        }
    }

    checkFilterChanged(e){
        console.log('filter changed event captuire ', e);
    }
}

```

### Docs in progress