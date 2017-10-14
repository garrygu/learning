## Articles
- [You Need ES2017’s Async Functions. Here’s Why](https://derickbailey.com/2017/04/18/you-need-es7s-async-functions-heres-why/?imm_mid=0f1021&cmp=em-web-na-na-newsltr_20170419)
- [Await and Async Explained with Diagrams and Examples (10/01/2017)](http://nikgrozev.com/2017/10/01/async-await/)

#### Using Async Functions

```
1 async function createEmployeeWorkflow(cb){
2   var err;
3

4   try {
5     var employee = await createEmployee();
6      
7     if (employee.needsManager()){
8       var manager = await selectManager(employee);
9       employee.manager = manager;
10     }
11      
12     await saveEmployee(employee);
13   } catch (ex) {
14     err = ex;
15   }
16

17   cb(err, employee);
18 }
```

#### Writing Async Functions
```
1 async function createEmployee(){
2   return new Promise((resolve, reject) => {
3      
4     // do stuff here to create the employee
5     var employee = // ...
6      
7     // now check if it worked or not
8     if (/* some success case */) {
9       resolve(employee);    
10     } else {
11       reject(someError);
12     }
13      
14   });
15 }
```

#### Async With Flexibility
```
1 function createEmployeeWorkflow(cb){
2    
3   createEmployee()
4     .then((employee) => {
5      
6       // ... all the other code
7      
8       cb(null, employee);
9      
10     }).catch((ex) {
11       return cb(ex);
12     });
13

14 }
```
