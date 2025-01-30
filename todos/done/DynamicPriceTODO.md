!TODO (DONE)

# Dynamic Pricing

## Requirements
- Being able to set dynamic pricing for services by employee or establishment wide.

## Concequences
- Appointments now need to store the pricing of the service when it was scheduled.
- List of services by establishment or employee need to take the dynamic pricing into account (Only possible to see if )
- List of employees might need to show the price of the service because it might differ from employee to employee.
- List of services by establishment need to be able to have highest and lowest price for each service(this might make the list of services by establishment a bit more complex/slower) (this should only be taken into account if we have a date selected)
- Alert we should somehow alert the user that the price of the service has been increase do to the amount of scheduled appointments for that day.

## Problems
- Design: 
    - We dont know the date that the service will be scheduled so we dont know if the price has been dynamicly changed.

### Possible solutions
- We could display the dates the price by near the date on the calendar. This would need to be designed in a way that it is readable and there should be a an alert on the day selection that for x reason the price of the service on this day will be X.
- We could mark the days that the price of the service has been increased with a different color on the calendar. And on the day selection we could show the price and reason for the price increase in a top banner.

## Explanation
Para termos precos a atualizar dinamicamente para certas datas ou com quando x amount of slots are available precisamos de mostrar ao cliente que esta mudanca de preco existe.
Como o nosso appointment flow é: 
Establishment selection -> Service selection -> Employee selection -> Date selection -> Time selection -> Confirmacao do appointment
Ou
Establishment selection -> Service selection -> Date selection -> Time selection -> Employee selection -> Confirmacao do appointment
Nós não sabemos a data que o servico vai ser agendado até ao passo de confirmacao do appointment. Por isso temos de mostrar ao cliente que o preco do servico foi alterado para o dia que ele esta a selecionar.
A unica ideia de como mostrar isto na minha opiniao é no calendario marcar os dias com diferentes precos com uma cor diferente para os clientes saberem que algo é diferente nesse dia. 
E quando tocam no dia mostrar um banner com o preco e a razao da mudanca de preco.

## TODO: 
- [ ] List of services when the day is available needs to take into account dynamic pricing
- [ ] On get available days of the month we need to return if there was a price increase
- [ ] A new alert that the selected day is under price increases (This can be achieved by the getAvailableDaysOfTheMonth and client side coding)
- [ ] There needs to be a way to set a % of appointments in a day from which the price will increase
- [ ] There should be a way to set a new price for the day by service and across services

## How to store
- [ ] New table
    - [ ] `EstablishmentServiceId`
    - [ ] `Price`
    - [ ] `DateTime-From`
    - [ ] `DateTime-To`
    - [ ] `Enabled`
    - [ ] `Establishment`
    - [ ] `Employee`
## How to retrieve
Get method that is called everytime the price is retrieved from the data base (this should be cached)
## Price needs to be saved by appointment
If the price can variate depending on the time that the appointment is made then the price needs to be saved in the appointment table
### Ways to implement retrival
1. Update all repository queries that retrive price
2. Create a new method that retrieves the price that is called by every service where the price is being returned
3. Create a generic interface/class that can be extended by the DTOs that need the price
4. Create a new class that will represent the prices and has a to string/to double method so that spring can create a json double from it and load a json double to it. Note that this needs to take mapper into account to see if it works
#### 1. Update all repository queries that retrive price
- [ ] List methods that retrieve price from DB
    - EstablishmentServiceRepository:
        - findAll
        - findByEstablishmentId
        - findAllServiceDTO
        - listServices
    - EstablishmentService
        - updateService
        - getService
- [ ] List DTOs that use price
    - EstablishmentServiceBaseDTO
    - EstablishmentServiceDTO
    - ServiceDTO
    - ServiceFullDTO
    - ServiceListDTO
    - CreateServiceDTO
    - CreateEstablishmentServiceDTO
#### 4. New price type
- [x] Create new class
- [x] See how to make json work with it as a double
- [x] See how to make modelMapper work with it as a double
- [ ] Create method that retrives and caches the actual price from the db
- [ ] Create set and get methods that uses new method to retrieve the price

## Front end methods that require the new price
- Get available days
- Get employee that can perform the service on a certain day
- Get available hours?
- On appointment creation

## Methods to change
- [x] EstablishmentsController (when date is available)
    - listEmployeesOfEstablishmentService
    - GetServices
- [x] ServicesController
    - listDynamicPrices
- [x] SchedulesController
    - ~~listSchedules~~ (This would make the request that is already "slow" even slower, so this can be used in combination with other requests to get the actual price for the given days [concurrently])
    - listSchedulesByDay
- [x] AppointmentsController (when the date/newDate is under dynamic price adjustments)
    - create

## How to retrieve the price
```sql
SELECT new com.teamsantos.easybarber.DTO.NameIdImagePriceDTO(
    e.employee.id,
    e.employee.employee.user.name,
    img.data,
    CASE
        WHEN dpe IS NOT NULL THEN dpe.price
        WHEN dp IS NOT NULL THEN dp.price
        ELSE e.service.price
    END
)
FROM EstablishmentServiceEmployee e
JOIN EmployeeSchedule es ON es.employee.id = e.employee.id AND es.establishment.id = e.establishment.id
LEFT JOIN e.employee.employee.images img ON img.isMain = true
LEFT JOIN e.dynamicPrices dpe ON dpe.from <= :date AND dpe.to >= :date
LEFT JOIN e.service.dynamicPrices dp ON dp.from <= :date AND dp.to >= :date
WHERE e.service.id = :establishmentServiceId
GROUP BY e.employee.id, e.employee.employee.user.name, img.data
```

## Client changes
- [x] List of employees when the day is already selected should show the updated price if that price has been updated
- [x] On get available days of the month we need to also make a request to know the days of the month that have had price adjustments
- [x] Paint the available days that have had price adjustments with a different color
- [x] Display a background color on the days in the calendar that have received a price adjustment
- [x] On a day selection that has had a price adjustment show a banner with the new price and the reason for the price adjustment
- [x] On scheduling a new appointment that has had a price adjustment show a alert that the price has been adjusted (message in the response) and ask for confirmation that the user still wants to keep the appointment (this should only happen on availabilityScreen, not on the employeeselection)
- [x] Implement way to create/delete/update dynamic prices
- [ ] Fix problems in tests
- [ ] Implement tests for the new functionality

## How do we want the price increases to beahave
- Price increases can be establishment wide or by employee. This means: 
    - When selecting the day and time of a service the employee might not yet be selected and therefor we might have price increases just for the employee that will endup being selected.
        This is hard to display on the application. 
        ### Solution
        If the employee is selected we get the establishment wide and employee price increases to alert the user, and display the special prices per employee on the employee selection screen., and display the special prices per employee on the employee selection screen., and display the special prices per employee on the employee selection screen., and display the special prices per employee on the employee selection screen.       If the employee is not selected we only get the establishment wide price increase to alert the user, and display the special prices per employee on the employee selection screen.

## Where was I:
Create new methods to create/delete/update dynamic prices