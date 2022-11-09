# Ticket Breakdown
We are a staffing company whose primary purpose is to book Agents at Shifts posted by Facilities on our platform. We're working on a new feature which will generate reports for our client Facilities containing info on how many hours each Agent worked in a given quarter by summing up every Shift they worked. Currently, this is how the process works:

- Data is saved in the database in the Facilities, Agents, and Shifts tables
- A function `getShiftsByFacility` is called with the Facility's id, returning all Shifts worked that quarter, including some metadata about the Agent assigned to each
- A function `generateReport` is then called with the list of Shifts. It converts them into a PDF which can be submitted by the Facility for compliance.

## You've been asked to work on a ticket. It reads:

**Currently, the id of each Agent on the reports we generate is their internal database id. We'd like to add the ability for Facilities to save their own custom ids for each Agent they work with and use that id when generating reports for them.**


Based on the information given, break this ticket down into 2-5 individual tickets to perform. Provide as much detail for each ticket as you can, including acceptance criteria, time/effort estimates, and implementation details. Feel free to make informed guesses about any unknown details - you can't guess "wrong".


You will be graded on the level of detail in each ticket, the clarity of the execution plan within and between tickets, and the intelligibility of your language. You don't need to be a native English speaker, but please proof-read your work.

## Your Breakdown Here
I'm creating the tickets from a SQL point of view

1.Update the Database tables Facilities, Agents, Shifts to support the new customId of Agent
    - Description: Add a new Column AGENT_ID and make it as primary key in Agents table and update the necessary foreign keys in Facilities and Shifts tables. 
    - Acceptance Criteria: The database should now support the new customId. While adding a new agent in AGENT table in database the AGENT_ID is mandatory without which you should not be able to save any agent detail. Also If one tries to save entries in FACILITIES or SHIFTS with invalid/incorrect AGENT_ID the date should not be saved instead an error should be thrown. Write unit test cases so that if some other developer changes the schema mistakenly he/she will get to know that they have make changes in the code as well.
    - Time estimate: 2 hours DEV + 2 hours TEST
    - Implementation details: use the ALTER table query to update the tables and also to add the necessary primary and foreign key contraints.

2. Update the code to handle the changes from database
    -Description: Add a new property agentId to Agent object and make sure that the same is referenced correctly from Facilities and Shifts objects. Write integration test cases to confirm the changes are working as expected.
    - Acceptance Criteria: The Object definitions of Shifts, Agents and Facilities should match with the tables SHIFTS, AGENS, FACILITIES in schema.
    - Time estimate: 3 hours DEV + 2 hours TEST
    - Implementation Details: First of all update the Agent object and then check all the places where this object is referenced and update code if the old ID is being used to the new agentId.
3. Update getShiftByFacility to return the agentId in agent metadata
    - Description: Update getShiftByFacility to return the agentId inside the metadata. Add unit test cases to verify the changes.
    - Acceptance Criteria: In the shifts array that getShiftByFacility returns, one should be able to see agentId property inside the metadata for each and every element inside the array. 
    - Time estimate: 1 hour DEV + 1 hour TEST
    - Implementation Details: Populate agentID inside agent metadata while populating the shift entries.
4. Update generateReport function to sum up hours based on the agent's new custom Id.
    - Description: This function should now sum up the shift hours by grouping the elements with same AgentId. Write unit test cases to verify the changes are working as expected.
    - Acceptance Criteria: The pdf should now show the hours against the new id of agent instead of the old id.
    - Time estime: 0.5 hours DEV + 1 hour TEST
    - Implementation details: Whilst processing the input we should relace the old id with agents newId.
