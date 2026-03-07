# Backend Architecture Checklist

Before completing backend tasks verify:

* [ ] routes only register endpoints
* [ ] handlers manage HTTP logic
* [ ] services contain business logic
* [ ] repositories manage persistence
* [ ] no direct database access in handlers
* [ ] services are functions
* [ ] repositories are functions
* [ ] no class keyword exists
* [ ] architecture follows routes → handlers → services → repositories