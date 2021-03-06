# Fake Valley Bank
A mock bank website built on the **C# ASP.NET Mvc** framework with a Code-First Database approach. Users have the ability to create an account in order to withdraw, deposit, and check balance of their **Fake Valley Bank** Account 

<img src="imgs/img 1.JPG" > 


## Models

### Customer Model

The bases of the application is the [Customer](https://github.com/ij2872/aspnet-FakeBank/blob/master/FakeNetBank/Models/Customer.cs) class.


##### SQL Table

| Id |  AccountNumber  |  FirstName  |  LastName  |  Balance  |  ApplicationUserId  |
|-|-|-|-|-|-|

```csharp
public class Customer
    {
        public int Id { get; set; }
        public string AccountNumber { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public decimal Balance { get; set; }
        public string Name
        {
            get
            {
                return string.Format("{0} {1}", this.FirstName, this.LastName);
            }
        }
        public virtual ApplicationUser User { get; set; }
        public string ApplicationUserId { get; set; }
        public virtual ICollection<Transaction> Transactions { get; set; }
    }
```

### Transaction Model

Withdraws and Deposits from a Customer are recorded using the [Transaction](https://github.com/ij2872/aspnet-FakeBank/blob/master/FakeNetBank/Models/Transaction.cs) Controller.

##### SQL Table

| Id |  Amount  |  CustomerId  | 
|-|-|-|

```csharp
    public class Transaction
    {
        public int Id { get; set; }

        [Required]
        [DataType(DataType.Currency)]
        public decimal Amount { get; set; }

        [Required]
        public int customerId { get; set; }

        public virtual Customer Customer { get; set; }
    }
```

### Creating and Registering a user 

[link to code](https://github.com/ij2872/aspnet-FakeBank/blob/5affebd5486d2c0001ef66c1d4635a24cc6f1cb3/FakeNetBank/Controllers/AccountController.cs#L161-L170)
```csharp

        UserManager.AddClaim(user.Id, new Claim(ClaimTypes.GivenName, model.FirstName));

        var db = new ApplicationDbContext();

        var accountNumber = (123459 + db.Customers.Count()).ToString().PadLeft(10, '0');
        var customer = new Customer { FirstName = model.FirstName, LastName = model.LastName,
                                      AccountNumber = accountNumber, Balance = 0, ApplicationUserId = user.Id};
        db.Customers.Add(customer);
        db.SaveChanges();
```
Account numbers are currently based of adding 123459 to the amount of customers located in the Customers Database Table. 
While this method works right now, it may lead to bugs in the future if (and when) users are deleted from the database. Therefore, a new accountNumber generating method will be implemented at a later date.


## Progressions 

#### 9-19-17

<img src="imgs/img 1.JPG" > 

<img src="imgs/img 2.JPG" >

Account page now shows table of user transactions using JQuery.DataTable.

#### 9-11-17


<img src="imgs/AppProgress1.0.JPG" >




#### Notes
At this moment, Users are able to withdraw and deposit. The homepage also allows for the use of Quick cash, with an AJax request to the [Transaction Controller](https://github.com/ij2872/aspnet-FakeBank/blob/master/FakeNetBank/Controllers/TransactionController.cs). Balance is viewable and obtained with an Ajax request, on load, to the [api/Customers](https://github.com/ij2872/aspnet-FakeBank/blob/master/FakeNetBank/Controllers/Api/CustomersController.cs) Controller.

---




## Author
   Ivan Jaimes
