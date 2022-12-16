# Lab №6. User types (range, enum)
---
### Task:
![The task](https://i.imgur.com/byicZlh.png)

#### Language: Delphi

### Algorithm scheme: 

![Algorithm scheme using the first variant Part1](https://i.imgur.com/fcUDTSn.png)

![Algorithm scheme using the first variant Part2](https://i.imgur.com/dNbwKgJ.png)

![Algorithm scheme using the first variant Part3](https://i.imgur.com/bYgn7E9.png)

![Algorithm scheme using the first vаriant Part4](https://i.imgur.com/SjqN24S.png)

![Algorithm scheme using the first variant Part5](https://i.imgur.com/XYTIIyL.png)

![Algorithm scheme using the first variant Part6](https://i.imgur.com/BhzHukb.png)

![Algorithm scheme using the first variant Part7](https://i.imgur.com/EMj3oMx.png)

### Code:
``` pascal
Program Lab6;
{
 Enter the time period (Day1, Month1, Year1 - Day2, Month2, Year2). Count and print Mondays which coming on the 13th
}

{$APPTYPE CONSOLE}

uses
  System.SysUtils;

Type
  Tday = 1..31;
  Tyear = 1..3000;
  Tmonth = (Jan = 1, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec);
  Tweek = (Monday = 1, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday);
  // Tday - allowed amount of days in a month
  // Tyear - allowable amount of years
  // Tmonth - available months
  // Tweek - available day of the week

Const

  // !Immutable constants
  Week = 7;
  FourWeeks = Week*4;
  FiveWeeks = Week*5;
  AmountDaysInMonth:Array [Tmonth] of Tday = (31,28,31,30,31,30,31,31,30,31,30,31);
  ArrMonth:Array [Ord(Low(Tmonth))..Ord(High(Tmonth))] of Tmonth = (Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec);
  MonthName:Array [Tmonth] of string = ('January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September',
                                        'October', 'November', 'December');
  DayOfWeekName: Array [Tweek] of string = ('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday');
  // Week - amount of days in a week
  // FourWeeks - amount of days in four weeks
  // FiveWeeks - amount of days in five weeks
  // AmountDaysInMonth - the element number corresponds to the month, and the content corresponds to the amount of days in the month
  // ArrMonth - the element number corresponds to the month number, and the content corresponds to months (of Tmonth)
  // MonthName - the element number corresponds to the month, and the content corresponds to the name of the month
  // DayOfWeekName - the element number corresponds to the day of the week, and the content corresponds to the name of the day of the week

  // !Mutable constants
  SearchedDayOfWeek = Monday;
  SearchedDayNum = 13;
  // SearchedDayOfWeek - searched day of the week
  // SearchedDayNum - the number of the searched day

Var
  Day1, Day2 : Tday;
  Month1, Month2 : Tmonth;
  Year1, Year2 : Tyear;
  CurrMonth : Tmonth;
  CurrDayNum, AmountDaysInCurrMonth, NumMonth : Byte;
  CounterAmountSDN, CurrYear: Word;
  flag, IsLeapYear : boolean;
  // Day1 - number for from the first date of the period
  // Day2 - number for from the second date of the period
  // Month1 - month for from the first date of the interval
  // Month2 - month for from the second date of the interval
  // Year1 - year for from the first date of the interval
  // Year2 - year for from the second date of the interval
  // NumMonth - entered month number
  // CounterAmountSDN - counter amount searched day number
  // CurrMonth - current month
  // CurrDayNum - current day number
  // AmountDaysInCurrMonth - amount of days in the current month
  // CurrYear - current year
  // flag - flag to confirm the correctness of entering numbers
  // IsLeapYear - flag to determine if a month is found by its number+

Begin

  Writeln('The day must match the month. The month must belong to the interval ',Ord(Low(Tmonth)),'..',Ord(High(Tmonth)),'.');
  Writeln('The year must belong to the interval ',Ord(Low(Tyear)),'..',Ord(High(Tyear)),'. Also, the second date must be greater than the first');
  Writeln('Enter your dates like this:');
  Writeln('day month year');
  Writeln('-');
  Writeln('day month year');
  Writeln;

  // Initialization CounterAmountSDN
  CounterAmountSDN:= 0;

  // Cycle with postcondition for entering correct data.
  repeat

    // Initialize the flag and IsLeapYear
    flag:= False;
    IsLeapYear:= False;

    // Validating the correct input data type
    Try
      Readln(Day1, NumMonth, Year1);
    Except
      Writeln('Incorrect type entry');
      flag:= True;
    End;

    // flag check
    if not flag then
    begin

      // Validate Range
      if not (Day1 in [Low(Tday)..High(Tday)]) or not (NumMonth in [Ord(Low(Tmonth))..Ord(High(Tmonth))])
         or (Year1 < Low(Tyear)) or (Year1 > High(Tyear)) then
      begin
        Writeln('Incorrect input! They dont match the required interval');
        flag:= True;
      end

      // Otherwise, check the correspondence between the number of days and the month
      else
      begin

        // Transfer from month number to month
        Month1:= ArrMonth[NumMonth];

        // Leap year definition
        IsLeapYear := (Year1 mod 4 = 0) and ((Year1 mod 100 <> 0) or (Year1 mod 400 = 0));

        // Check the correspondence between the number of days and the month
        if IsLeapYear and (Month1 = Feb) and (Day1 > AmountDaysInMonth[Month1] + 1) then
        begin
          Writeln('Since ',Year1,' is a leap year, ',MonthName[Month1],' has ',AmountDaysInMonth[Month1] + 1,' days');
          flag:= True;
        end
        else if Day1 > AmountDaysInMonth[Month1] then
        begin
          Writeln(MonthName[Month1],' has ',AmountDaysInMonth[Month1],' days');
          flag:= True;
        end;

      end;
    end;

  until not flag;

  Writeln('-');

  // Cycle with postcondition for entering correct data.
  repeat

    // Initialize the flag and IsLeapYear
    flag:= False;
    IsLeapYear:= False;

    // Validating the correct input data type
    Try
      Readln(Day2, NumMonth, Year2);
    Except
      Writeln('Incorrect type entry');
      flag:= True;
    End;

    // flag check
    if not flag then
    begin

      // Validate Range
      if not (Day1 in [Low(Tday)..High(Tday)]) or not (NumMonth in [Ord(Low(Tmonth))..Ord(High(Tmonth))])
         or (Year1 < Low(Tyear)) or (Year1 > High(Tyear)) then
      begin
        Writeln('Incorrect input! They dont match the required interval');
        flag:= True;
      end

      // Otherwise, check the correspondence between the number of days and the month
      else
      begin

        // Transfer from month number to month
        Month2:= ArrMonth[NumMonth];

        // Leap year definition
        IsLeapYear:= (Year2 mod 4 = 0) and ((Year2 mod 100 <> 0) or (Year2 mod 400 = 0));

        // Check the correspondence between the number of days and the month
        if IsLeapYear and (Month2 = Feb) and (Day2 > AmountDaysInMonth[Month2] + 1) then
        begin
          Writeln('Since ',Year2,' is a leap year, ',MonthName[Month2],' has ',AmountDaysInMonth[Month2] + 1,' days');
          flag:= True;
        end
        else if Day1 > AmountDaysInMonth[Month1] then
        begin
          Writeln(MonthName[Month2],' has ',AmountDaysInMonth[Month2],' days');
          flag:= True;
        end

        // Checking if the second date is greater than the first
        else if (Year2 < Year1) or ((Year2 = Year1) and (Ord(Month2) < Ord(Month1)))
             or ((Year2 = Year1) and (Ord(Month2) = Ord(Month1)) and (Day2 < Day1)) then
        begin
          Writeln('Time interval is wrong! The second data is greater than the first');
          flag:= True;
        end;

      end;
    end;

  until not flag;

  // Decide on which days of the week will search for the dates of the days
  CurrDayNum:= Ord(SearchedDayOfWeek);

  // Initialize IsLeapYear
  IsLeapYear:= False;

  // Starting from the year (in which first of January is Monday), iterate through all the years
  // up to the firs entered year (In this cycle, find the first SearchedDayOfWeek in Month1 Year1)
  for  CurrYear:= (400 * ((Year1-1) div 400) + 1) to Year1 do
  begin

    // Reset CurrMonth to the first month
    CurrMonth:= Low(Tmonth);

    // Leap year definition
    IsLeapYear:= (CurrYear mod 4 = 0) and ((CurrYear mod 100 <> 0) or (CurrYear mod 400 = 0));

    // Iterate over all possible months or, if it is Year1, before the first entered month
    while (CurrMonth <= High(Tmonth)) and ((CurrYear <> Year1) or (CurrMonth < Month1)) do
    begin

      // Found the amount of days in the current month
      AmountDaysInCurrMonth:= AmountDaysInMonth[CurrMonth];
      if IsLeapYear and (CurrMonth = Feb) then
        Inc(AmountDaysInCurrMonth);

      // Depending on the amount of days between the current day and the amount of days
      // in the current month, find the date of the first SearchedDayOfWeek of the next month
      if AmountDaysInCurrMonth - CurrDayNum + 1 > FourWeeks then
        CurrDayNum:= FiveWeeks - AmountDaysInCurrMonth + CurrDayNum
      else
        CurrDayNum:= CurrDayNum - (AmountDaysInCurrMonth - FourWeeks);

      // Find the current month for which found the date of the first SearchedDayOfWeek
      CurrMonth:= Succ(CurrMonth);
    end;

  end;

  // In this cycle, find the firts SearchedDayOfWeek after Day1
  while CurrDayNum < Day1 do
    Inc(CurrDayNum, Week);

  Writeln;
  Writeln('Found the following data coming on ', DayOfWeekName[SearchedDayOfWeek],' the ',SearchedDayNum,'th:');

  // Iterate from year to year 2 (in this cycle find all SearchedDayNum SearchedDayOfWeek
  for CurrYear := Year1 to Year2 do
  begin

    // Leap year definition
    IsLeapYear:= (CurrYear mod 4 = 0) and ((CurrYear mod 100 <> 0) or (CurrYear mod 400 = 0));

    // Iterate over all possible months or, if it is Year2, before the second entered month
    while (CurrMonth <= High(Tmonth)) and ((CurrYear <> Year2) or (CurrMonth <= Month2)) do
    begin

      // Found the amount of days in the current month
      AmountDaysInCurrMonth:= AmountDaysInMonth[CurrMonth];
      if IsLeapYear and (CurrMonth = Feb) then
        Inc(AmountDaysInCurrMonth);

      // Iterate over all possible months or, if it is Year2 and Month2, before the second entered month
      while (CurrDayNum <= AmountDaysInCurrMonth) and ((CurrYear <> Year2) or (CurrMonth <> Month2) or (CurrDayNum <= Day2)) do
      begin

        // The current number corresponds to SearchedDayNum, then display the current date
        if CurrDayNum = SearchedDayNum then
        begin
          writeln(CurrDayNum,' ',Ord(CurrMonth),' ',CurrYear);
          Inc(CounterAmountSDN);
        end;

        // Incrementing the current date by a week
        Inc(CurrDayNum, Week);
      end;

      // Finding the first SearchedDayOfWeek in the next month
      CurrDayNum:= CurrDayNum - AmountDaysInCurrMonth;

      // Modernize CurrMonth
      CurrMonth:= Succ(CurrMonth);
    end;

    // Reset CurrMonth to the first month
    CurrMonth:= Low(Tmonth);
  end;

  Writeln('Finally found ',CounterAmountSDN,' dates');

  Readln;
  Readln;
End.
```
