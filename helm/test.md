# Testing Services

All of the services developed for the platform should pass basic acceptance tests before they are deployed. 
The tests should verify the core functionality of the service. The tests should catch configuration issues or problems with the service and ensure that the service functions as intended. 

Generally the tests are located in a Helm proxy chart for the service. The tests are located in a folder called `test`. Before any merge of changes onto the main branch, a github workflow automatically runs the test in a `kind` cluster. 
