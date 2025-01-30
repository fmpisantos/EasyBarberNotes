!TODO

# Product suggestions

## Tables
- [ ] Product
    - [ ] Id
    - [ ] ProductTypeIds(by default all products belong to ALL type: [1])
    - [ ] EstablishmentId
    - [ ] EmployeeId (non required)
    - [ ] Name
    - [ ] Description
    - [ ] Price
    - [ ] Quantity (non required)
    - [ ] Id
- [ ] ProductType
    - [ ] Name
    - [ ] Description
    - [ ] ParentProductTypeIds (non required)
- [ ] ProductSuggestions
    - [ ] ProductId
    - [ ] ClientId
- [ ] ProductRequests
    - [ ] AppointmentId
    - [ ] ProductIds

## Requests
- [ ] List suggestions by client
- [ ] Add suggestion to client
- [ ] List products (filtered by type, establishment, employee, price, ammount, productType)
- [ ] Add product
- [ ] Remove product
- [ ] Disable product
- [ ] Request product
    - [ ] List product requested by appointment, establishment, employee