# Data processing using Step Function

## Overview
In this project, I created a Data processing workflow using step function. This Architecture be used in many places and one of it best use if customer data flow. We are processing the data and saving the in dynamoDB and also deleting the data if it is required.


## Architecture

I created a Step Function state machine that defines the sequence of steps in your workflow. The workflow will iterate over all records in a CSV file stored somewhere but in this project We are sending this data by ourself. For each record, I
process the contents and save the data into a database, in this DynamoDB. Optionally, Any other database can also be used. Once the batch of data is processed, We can check go for checking credit card transaction. Uf the transaction is failed then we will delete the customer data if the it is success then it will move ahead and we can do any process we want like send SMS or email to customer.

AWS services used

- Step function
- Lambda
- DynamoDB

![image](https://github.com/Abhishek9821228/CarHub/assets/135989734/c76fa916-8c14-4e2b-88e0-0f70a073d479)

# Step 1 - Creating a DynamoDb table for storing the data

In this step, I used AWS dynamoDB to create a customerOrderTable to insert the data of customer.

    1. Go to dynamoDB Console.
    2. Create Table named "customerOrderTable".
    3. Give partition Key to customerID and sort key to "orderID".


![image](https://github.com/Abhishek9821228/CarHub/assets/135989734/983883f2-2a31-40ca-94d0-647acf0a8c43)


# Step 2 - Now Create a AWS Lambda function.

In this step, I created Lambda function which will be getting invoked by users once the data was successfully put into the dynamoDB Table. 

    1. Create a lambda fucntion.
    2. Write function for it to calculate the result.
    3. Test the function.


![image](https://github.com/Abhishek9821228/CarHub/assets/135989734/14523a7c-8fd7-4c97-9124-a725cc858275)


# Step 3 - Create the step function.

In this step, I use AWS step function to start by send and puting the data into the dynamoDB and then using AWS lambda to see if the transaction was successfull and then deciding the success and failure of the flow. 

    1. Create a step function.
    2. Use dynamoDb put flow and insert it after the start.


![image](https://github.com/Abhishek9821228/CarHub/assets/135989734/59565ae5-fe70-4998-8517-35feb5e3c643)

    3. Now we have to check it the insertion of data was successfull. For this we used the choice action and give one of the condition to be if the "HttpsStatuscode" is 200. This is automatically generated in the output.
    4. IF it goes wrong then we will send the flow to fail action.


![image](https://github.com/Abhishek9821228/CarHub/assets/135989734/3aa37632-2bac-45f1-a8e8-31c43ed20cc5)

![image](https://github.com/Abhishek9821228/CarHub/assets/135989734/d1f21332-98d9-4c59-aaa8-4d4a7eaff2a8)

    5. Now if the we will use the lamda fucntion to check this the transaction is success if it is then we will send the flow to success action. If it is not we will use the dynamoDB dete flow to remove the customer details. And send the data to fail action.


![image](https://github.com/Abhishek9821228/CarHub/assets/135989734/c76fa916-8c14-4e2b-88e0-0f70a073d479)

![image](https://github.com/Abhishek9821228/CarHub/assets/135989734/f0dc580e-856d-4d2e-b059-12e1b9bf7ebf)

## conclusion

We create a Data flow model successfully and the test on it was also successfull.
