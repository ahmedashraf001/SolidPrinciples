
### 1.  **SRP Single responsibility Solid Principle**:
    
# Violate SRP : 

```c#
public class Invoice
{
	public void GenerateInvoice()
	{
	}

	public void PrintInvoice()
	{
	}

	public void SaveToDatabase()
	{
	}

	public void SendEmail(string email)
	{
	}
}
```

# Follow SRP : 
```c#
public class Invoice
{
	public class InvoiceCreation
	{
		public void GenerateInvoice()
		{
		}
	}

	public class InvoicePrint
	{
		public void PrintInvoice()
		{
		}

	}

	public class InvoiceSaving
	{
		public void SaveToDatabase()
		{
		}
	}


	public class EmailSending
	{
		public void InvoiceEmail(string email , Invoice invoice)
		{
		}
	}
	
}
```	


### 2.  **OCP open closed principle Solid Principle**:
    
# Violate OCP : 

```c#
public class Employee
{
	public int Id { get; set; }	
	public string Name { get; set; }	
	public decimal BasicSalary { get; set; }	

	public decimal CalcHoursBonus(decimal ExtraHours)
	{
		// method code that may need modifications or Extentions in the future depend on Employee JobTitles
		return 0m;
	}

    public void anyMethod()
	{
		// code may need further modifications or Extentions in the future
	}

}
```
 
# Follow OCP : 
```c#
public abstract class Employee
{
	public int Id { get; set; }	
	public string Name { get; set; }	
	public decimal BasicSalary { get; set; }

	public abstract decimal CalcHoursBonus(decimal ExtraHours);

	public abstract void anyMethod();
	 
}


public class OfficeBoyEMP : Employee
{
	
	public override decimal CalcHoursBonus(decimal ExtraHours)
	{
		// specific implementation code
		return 0m;
	}

	public override void anyMethod()
	{
		// specific implementation code
	}

}

public class developerEMP : Employee
{

	public override decimal CalcHoursBonus(decimal ExtraHours)
	{
		// specific implementation code
		return 0m;
	}

	public override void anyMethod()
	{
		// specific implementation code
	}

}


public class Program
{
	static void Main(string[] args)
	{

		// different CalcHoursBonus(); implementations depending on the Employee

		OfficeBoyEMP ob = new OfficeBoyEMP();
		Console.WriteLine(ob.CalcHoursBonus(10));

		developerEMP obb = new developerEMP();
		Console.WriteLine(obb.CalcHoursBonus(10));

	}
}
```	


### 3.  **LSP Liskov Substitution Solid Principle**:
    
# Violate LSP: 

```c#
public class Employee
{
	public int Id { get; set; }	
	public string Name { get; set; }	
	public decimal BasicSalary { get; set; }	

	public decimal CalcHoursBonus(decimal ExtraHours)
	{
		// method code that may need modifications or Extentions in the future depend on Employee JobTitles
		return 0m;
	}
 
}
```

# Follow LSP: 
```c#
  class Employee
	{
		public int Id { get; set; }	
		public string Name { get; set; }	
		public decimal BasicSalary { get; set; }

		public  virtual decimal CalcHoursBonus(decimal ExtraHours)
		{
			// code for default employee
			return 0m;
		}


	}

	public class OfficeBoyEMP : Employee
	{
		
		public override decimal CalcHoursBonus(decimal ExtraHours)
		{
			// specific implementation code
			return 0m;
		}

	}

	public class developerEMP : Employee
	{

		public override decimal CalcHoursBonus(decimal ExtraHours)
		{
			// specific implementation code
			return 0m;
		}

	}

	public class Program
	{
		static void Main(string[] args)
		{

			// different CalcHoursBonus(); implementations depending on the Employee

			Employee ob = new OfficeBoyEMP();
			Console.WriteLine(ob.CalcHoursBonus(10));

			Employee obb = new developerEMP();
			Console.WriteLine(obb.CalcHoursBonus(10));

		}
	}
```	



### 4.  **ISP Interface Segregation solid Principle**:
    
# Violate ISP : 

```c#
public interface IPrinter
{
	void Print(string content);
	void Scan(string content);
	void Fax(string content); 
}

// SimplePrinter can do only Print method , but it has to implement all methods
public class SimplePrinter : IPrinter
{
	public void Fax(string content)
	{
		// implementation code
	}

	public void Print(string content)
	{
		// implementation code
	}

	public void Scan(string content)
	{
		// implementation code
	}


}
```

# Follow ISP : 
```c#
public interface IPrint
{
	void Print(string content);
}

public interface IScan
{
	void Scan(string content);
}

public interface IFax
{
	void Fax(string content);
}

// SimplePrinter can do only Print method 
public class SimplePrinter : IPrint
{
	public void Print(string content)
	{
		// implementation code
	}
}

// AdvancedPrinter can print and scan
public class AdvancedPrinter : IPrint, IScan
{
	public void Print(string content)
	{
		// implementation code
	}

	public void Scan(string content)
	{
		// implementation code
	}
}
```	


### 5.  **DIP dependency inversion Solid Principle**:
    
# Violate DIP: 

```c#
public class gmail
{
	//member01
	//member02
	public void Send() { /* sending gmail implementation */ }
}
public class Hotmail
{
	//member01
	//member02
	public void Send() { /* sending Hotmail implementation */ }
}

public class yahoo
{
	//member01
	//member02
	public void Send() { /* sending yahoo implementation */ }
}


// class depend directly on another small classes , so you will have to modify this class if you need to add other new small classes
public class Notifications
{
	public gmail gmail = new gmail();
	public Hotmail hotmail = new Hotmail();	
	public yahoo yahoo = new yahoo();	
}
```

# Follow DIP: 
```c#
public interface IMessage
{
	public void Send();
}

public class gmail : IMessage
{
	//member01
	//member02
	public void Send() { /* sending gmail implementation */ }
}
public class Hotmail : IMessage
{
	//member01
	//member02
	public void Send() { /* sending Hotmail implementation */ }
}

public class yahoo : IMessage
{
	//member01
	//member02
	public void Send() { /* sending yahoo implementation */ }
}


// class depend indirectly on another small classes ,
// Notifications class depends on the IMessage interface as a broker.
public class Notifications
{
	public IMessage message;

	public Notifications(IMessage message)
	{ 
		this.message = message;
	}
	public void NotificationSend() { message.Send(); }	
}

public class Program
{
	static void Main(string[] args)
	{
		// send notification via gmail in the program
		Notifications ob = new Notifications(new gmail());
		ob.NotificationSend();
	}
}
```	
