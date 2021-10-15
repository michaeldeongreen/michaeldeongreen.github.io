---
layout: post
title:  "Re-factoring Legacy Code"
date:  2018-03-02
categories: [technology]
images: re-factoring-legacy-code-001.png
tags: [Re-factoring, Legacy Code]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/re-factoring-legacy-code-001.png)

Lately, I have been working with a lot of legacy code. One of the hallmarks of legacy code is that it often times contains mission critical code, it isn't Unit Testable and because of the aforementioned reasons, developers don't enjoy making changes to it.  
  
I wanted to take some time to talk about how I tend to approach re-factoring legacy code and provide a recent example of how I partially re-factored legacy code in a mission critical application.  
  
**How I Re-factor Legacy Code!**  
  
When deciding to re-factor legacy code, I tend to ask myself a few questions:  

* **Is the code Unit Testable?** - If Unit Tests don't exist, that could be a warning sign that the code is buggy!
* **Is the code touched frequently?** - If there are no Unit Tests and the code changes often, this triggers more warning signs!
* **Is the code difficult to modify in its current form?** - If there are no Unit Tests, the code changes often and the code is difficult to maintain, then Houston we have a problem!
* **To a lesser degree, is their time to re-factor the code?** - There is always time to re-factor :)!

Below is a summarized version of the class before I re-factored a portion of the class, chiefly, the _Reload_ Method. Also note, that I am using generic names and have removed most of the code for brevity and I not the owner of the code.  
  
```csharp
    
        public class LoanApplication
        {
      public int ApplicationId { get; set; }
      public Entity1 Entity1 { get; set; }
      //....
      
      public LoanApplication()
      {
       //....
      }
      
      public void BusinessLogicMethod1()
      {
       //....
      }
      
      public void BusinessLogicMethod2()
      {
       //....
      }  
      
      public void BusinessLogicMethod2()
      {
       //....
      }  
      
      public void Encrypt()
      {
       //....
      }  
    
      public void Decrypt()
      {
       //....
      }    
      
            public LoanApplication Reload()
            {
                var dataSet = GetLoanApplicationFromDb(ApplicationId);
    
                if (dataSet.Tables.Count == 0 || dataSet.Tables[0].Rows.Count == 0) return this;
    
                int tableIndex = 0;
                DataTable currentTable = null;
                DataRow mainRow;
    
                //------------------------------------------------------------------------------------------
                // Table[0]
                //------------------------------------------------------------------------------------------
    
                currentTable = dataSet.Tables[tableIndex++];
                if (currentTable.Rows.Count > 0)
                {
        //....Set Entity1 properties
                }
    
                //------------------------------------------------------------------------------------------
                // Table[1]
                //------------------------------------------------------------------------------------------
                
       currentTable = dataSet.Tables[tableIndex++];
                if (currentTable.Rows.Count > 0)
                {
        //....
                }
    
                //------------------------------------------------------------------------------------------
                // Table[2]
                //------------------------------------------------------------------------------------------
    
                currentTable = dataSet.Tables[tableIndex++];
                if (currentTable.Rows.Count > 0)
                {
        //....
                }
    
                //------------------------------------------------------------------------------------------
                // Table[3]
                //------------------------------------------------------------------------------------------
       
                currentTable = dataSet.Tables[tableIndex++];
       if (currentTable.Rows.Count > 0)
                {
        //....
                }
    
                //------------------------------------------------------------------------------------------
                // Table[4]
                //------------------------------------------------------------------------------------------
       
       currentTable = dataSet.Tables[tableIndex++];
                if (currentTable.Rows.Count > 0)
                {
        //....
                }
    
                //------------------------------------------------------------------------------------------
                // Table[5] 
                //------------------------------------------------------------------------------------------
       
       currentTable = dataSet.Tables[tableIndex++];
                if (currentTable.Rows.Count > 0)
                {
        //....
                }
    
                //------------------------------------------------------------------------------------------
                // Table[6-10]
                //------------------------------------------------------------------------------------------
    
       //....Logic for Table[6] thru Table[10], which are sequentially dependent on each other
    
                //------------------------------------------------------------------------------------------
                // Table[11]
                //------------------------------------------------------------------------------------------
       
       currentTable = dataSet.Tables[tableIndex++];
                if (currentTable.Rows.Count > 0)
                {
        //....
                }
       
                //------------------------------------------------------------------------------------------
                // Table[12]
                //------------------------------------------------------------------------------------------
       
       currentTable = dataSet.Tables[tableIndex++];
                if (currentTable.Rows.Count > 0)
                {
        //....
                }   
    
                return this;
            }
     }
    
```
  
**Pre-Re-factor Overview!**  
  
The LoanApplication _Reload_ method is called to load Loan Application Data. Inside of this method, a Stored Procedure is executed that returns the results of N number of SELECT statements and a DataSet is hydrated with these records. The code uses the tableIndex variable to maintain the DataSet index position, increment this variable, get the DataTable and perform logic and set LoanApplication properties such as Entity1.  
  
The order in which these DataTables are processed matters and Datatables 6 thru 10 are processed in a fairly large and complicated code block.  
  
If a developer needed to add DataTable 13, they would need to:  

* Modify the Stored Procedure
* Try to comprehend how the existing code works, which is not very intuitive
* Increment the tabindex variable
* Write the code for DataTable 13
* Understand that the code they wrote is not Unit Testable
* Understand that the only way to test the code is to run all applications that use this code
* Understand that they didn't really follow the [The Boy Scout Rule](http://programmer.97things.oreilly.com/wiki/index.php/The_Boy_Scout_Rule)

**Code Smell!**  
  
* The code is not Unit Testable
* The code is hard to follow
* The code is somewhat difficult to modify

**Time to Re-factor!**  
  
```csharp
     //....Real enum names replaced with generic Entity# name
     public enum LoanApplicationTable : byte
     {
      Entity1 = 0,
      Entity2 = 1,
      Entity3 = 2,
      Entity4 = 3,
      Entity5 = 4,
      Entity6 = 5,
      Entity7 = 6,
      Entity8 = 7,
      Entity9 = 8,
      Entity10 = 9,
      Entity11 = 10,
      Entity12 = 11
     }
    

  
  

    
     //...Extension Method
     public static int ToInt(this LoanApplicationTable value)
     {
      return (int)value;
     }
    

  
  

    
     //....Interface
        public interface ILoanApplicationLoadService
        {
            void Load(LoanApplication loanApplication, DataSet dsLoanApplicationData);
        }
    

  
  

    
     //...Extension Method
        public class LoanApplicationLoadService : ILoanApplicationLoadService
        {
            private LoanApplication _loanApplication;
            private DataSet _dsLoanApplicationData;
            public void Load(LoanApplication loanApplication, DataSet dsLoanApplicationData)
            {
                if (dsLoanApplicationData == null || dsLoanApplicationData.Tables.Count == 0 || dsLoanApplicationData.Tables[0].Rows.Count == 0) return;
    
                _loanApplication = loanApplication;
                _dsLoanApplicationData = dsLoanApplicationData;
    
                LoadEntity1();
                LoadEntity2();
                LoadEntity3();
                LoadEntity4();
                LoadEntity5();
                LoadEntity6();
                LoadEntities7thru10();
                LoadEntity11();
                LoadEntity12();
            } 
      
            private void LoadEntity1()
            {
                DataTable currentTable = GetLoanApplicationTable(LoanApplicationTable.Entity1);
                DataRow mainRow = null;
    
                if (currentTable.Rows.Count > 0)
                {
        //....
                }
            }  
      
      //.....Load other entities
      
            private DataTable GetLoanApplicationTable(LoanApplicationTable type)
            {
                return _dsLoanApplicationData.Tables[type.ToInt()];
            }  
        }
    

  
  

    
     //....Re-factored LoanApplication
        public class LoanApplication
        {
      public int ApplicationId { get; set; }
      public Entity1 Entity1 { get; set; }
      public readonly ILoanApplicationLoadService _loanApplicationLoadService;
      //....
      
      public LoanApplication(ILoanApplicationLoadService loanApplicationLoadService)
      {
       _loanApplicationLoadService = loanApplicationLoadService;
       //....
      }
      
      public void BusinessLogicMethod1()
      {
       //....
      }
      
      public void BusinessLogicMethod2()
      {
       //....
      }  
      
      public void BusinessLogicMethod2()
      {
       //....
      }  
      
      public void Encrypt()
      {
       //....
      }  
    
      public void Decrypt()
      {
       //....
      }    
      
            public LoanApplication Reload()
            {
                var dataSet = GetLoanApplicationFromDb(ApplicationId);
    
                if (dataSet.Tables.Count == 0 || dataSet.Tables[0].Rows.Count == 0) return this;
    
                _loanApplicationLoadService.Load(this, dataSet);
    
                return this;
            }
     }
    

```  
  
**Post-Re-factor Overview!**  
  
[Martin Fowler](https://en.wikiquote.org/wiki/Martin_Fowler) once stated, "Any fool can write code that a computer can understand. Good programmers write code that humans can understand". I am a firm believer in this quote and I always try to keep it in mind when writting code.  
  
First, I created a enum to contain the actual Table names of the DataTables that are returned from the Stored Procedure (Again, I removed the actual names and used Entity#). I believe it makes the code more readable and when another table is added, the developer will intuitively know to just add another name to the enum with the correct integer value.  
  
Next, I decided to make an Extension Method for the new enum to easily convert the enum to its integer value, which would be needed as the code accesses each index in the DataSet.  
  
I have been pushing the use of [Structuremap](http://structuremap.github.io/) at my current client and adoption has been extremely rapid. I decided to create the ILoanApplicationLoadService service and inject it into the LoanApplication class constructor to remove tight coupling and to get one step closer to making the LoanApplication class Unit Testable.  
  
After creating the inteface, I created the LoanApplicationLoadService, which implements ILoanApplicationLoadService. A few points:  
  
* I intentionally left the Stored Procedure call in the LoanApplication class to make Unit Testing possible
* GetLoanApplicationTable Method makes use of the Extension Method I created
* In each Load Method, I check to see if there are any rows in their corresponding DataTable, which will aid in Unit Testing

Last but not least, I made the appropriate modification in the LoanApplication class. I created a new readonly property of type ILoanApplicationLoadService and I am using constructor injection to set this property. The _Reload_ method has been reduced to only a few lines of code.  
  
If a developer needed to add DataTable 13 after the re-factor, they would need to:  

* Modify the Stored Procedure
* Try to comprehend the LoanApplicationLoadService class, which I believe is more intuitive
* Add a new value to the LoanApplicationTable enum
* Write the code for DataTable 13
* Write Unit Tests to support the new code (If doing [TDD](https://en.wikipedia.org/wiki/Test-driven_development), this would occur much earlier in the process)

**Sample Unit Tests**  
  
```csharp
     //....LoanApplicationTable enum Extension Method Unit Tests
     [Test]
     public void ToInt_WhenEntity1Enum_ReturnsZero()
     {
      //given
      LoanApplicationTable table = LoanApplicationTable.Entity1;
      //when
      int value = table.ToInt();
      //then
      value.Should().Be(0);
     }
     
     //....
     
     //....LoanApplicationLoadService Unit Tests
     [Test]
     public void LoanApplicationLoadService_WhenEntity1Name_ReturnsName()
     {
      //given
      DataSet dsLoanApplicationData = GetEntity1OnlyMockData();
      LoanApplication loanApplication = GetNewLoanApplication();
      ILoanApplicationLoadService service = new LoanApplicationLoadService();
      //when
      service.Load(loanApplication, dsLoanApplicationData); //...Only LoadEntity1 Method will run because the other DataTables will not have records
      //then
      loanApplication.Name.Should().Be("MockDataEntity1NameValue");
     } 
     
     //....
```
