# Booking System – individual coding exam

This is a starter project for the individual practical coding exam. The
application targets .NET 10 and contains a console application and xUnit tests.

I have tried to keep the structure intact, meaning ConsoleUI handles the menu while BookingService handles the business logic and FileBookingRepository the file storing/loading.

## Requirements

- .NET 10 SDK

## Run the application

### Via Visual Studio
1. Open the solution in visual studio. 
1. Set BookingSystem as starting project if it's not already enabled as.
3. Press F5 or click the green arrow start button. 

### Via terminal
```bash
dotnet run --project src/BookingSystem
```

## Run the tests

### Via Visual Studio
1. Click on Test dropdown menu.
2. Selected "Test Explorer".
3. Click "Run all Tests".

### Via terminal
```bash
dotnet test
```


