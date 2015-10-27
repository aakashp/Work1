void Customer::viewroomdetails()
{
  if(connection()) 
  { 
   EXEC SQL DECLARE Curs CURSOR for select * from room_details where available="available"; 
   EXEC SQL OPEN Curs; 
   if(sqlca.sqlcode <0) 
   { 
    cout<<"Error in Execution"; 
   } 
   for(;;) 
   { 
    EXEC SQL FETCH Curs into :d_room_no,:d_room_type,: 
    d_tariff,:d_available; //declare these local var later
    if(sqlca.sqlcode == NODATAFOUND) break; 
    if(sqlca.sqlcode <0) 
    { 
     cout<<"Error in Execute"; 
    } 
    cout<<"***Room Details***"<<endl; 
    cout<<"Room No : "<<d_room_no<<endl;//int 
    cout<<"Room Type : "<<d_room_type<<endl; //varchar
    cout<<"Tariff: "<<d_tariff<<endl; //int
    cout<<"Availability : "<<d_available<<endl;//varchar 
    } 
    cout<<"******************"<<endl; 
    EXEC SQL COMMIT WORK RELEASE; 

    }
    else
    {
      cout<<"Connection failed"; 
    }
}



void customer::book(double room_id)// ask customer which room_no he wants in main
{
if(connection()) 
  { 
   d_room_no = room_id;
   EXEC SQL update table room_details set available="booked" where room_no =:d_room_no; 
    
   if(sqlca.sqlcode <0) 
   { 
    cout<<"Error in Execution"; 
   } 
   EXEC SQL COMMIT WORK RELEASE; 
   




}

manager.hpp
#ifndef manager_hpp
#define manager_hpp

class manager:public customer 
{
  public:
    bool connection();//dont think this is need since inheritance is there????
    void view();
    void payment(double);//either payment() or book() must be used since in payment we could have all func of book plus caln part and sql part of adding data to another table of view() can be added.
    
