# Streamlined-Employee-Detail-Management

Overview of the project 

Our Employee Detail Management System uses CRM technology to store and manage employee information in one place. 
This makes it easier for HR and managers to access and update employee data, saving time and increasing efficiency.

Apex Class code:
public class LeaveTriggerHandler {
    

              public static void ifMaleEmployee(List<Leave__c> leaveRequests) {

              // Fetch employees related to leave requests

              Set<Id> employeeIds = new Set<Id>();

               for (Leave__c leaveRequest : leaveRequests) {

               if (leaveRequest.Employee__c != null) {

                

                employeeIds.add(leaveRequest.Employee__c);

            }

        }


        // Fetch employee records

        Map<Id, Employee__c> employeesMap = new Map<Id, Employee__c>([SELECT Id, Gender__c FROM Employee__c WHERE Id IN :employeeIds]);


        // Check eligibility for maternity leave and gender

        for (Leave__c leaveRequest : leaveRequests) {

            if (leaveRequest.Leave_Type__c == 'Maternity Leave') {

                Employee__c emp = employeesMap.get(leaveRequest.Employee__c);

                if (emp != null && emp.Gender__c != null && emp.Gender__c == 'Male') {

                    leaveRequest.addError('Male employees are not eligible for Maternity Leave');

                }

            }

        }

    }

}

Apex Trigger code:
trigger LeaveTrigger on Leave__c (before insert) {
 if(trigger.isBefore){
  if(trigger.isInsert){
    LeaveTriggerHandler.ifMaleEmployee(trigger.new); 
       }
     }
}
