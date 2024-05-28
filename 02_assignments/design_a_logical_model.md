# Assignment 1: Design a Logical Model

## Question 1
Create a logical model for a small bookstore. 📚

At the minimum it should have employee, order, sales, customer, and book entities (tables). Determine sensible column and table design based on what you know about these concepts. Keep it simple, but work out sensible relationships to keep tables reasonably sized. Include a date table. There are several tools online you can use, I'd recommend [_Draw.io_](https://www.drawio.com/) or [_LucidChart_](https://www.lucidchart.com/pages/).

![image](https://github.com/monzchan/SQL/assets/166637673/58d3bfe0-593a-41f6-a013-e500e0f7ace1)

## Question 2
We want to create employee shifts, splitting up the day into morning and evening. Add this to the ERD.

![image](https://github.com/monzchan/SQL/assets/166637673/f895c06b-c7c0-4d4a-9d9a-654f32519e21)

## Question 3
The store wants to keep customer addresses. Propose two architectures for the CUSTOMER_ADDRESS table, one that will retain changes, and another that will overwrite. Which is type 1, which is type 2?

_Hint, search type 1 vs type 2 slowly changing dimensions._

Type 1
 ![image](https://github.com/monzchan/SQL/assets/166637673/5a755a5a-1705-4b22-808f-e00bc37fe8d8)

Type 2 
 ![image](https://github.com/monzchan/SQL/assets/166637673/0b1a0ced-c5d7-4367-8318-8c14d15b72d0)


For type 2 architecture, privacy issue raised since customer data will be retained in the data bank forever. It enables data scientist to track historically significant attributes. But for type 1 slowly changing dimension, the data about customer’s information will be overwritten by updated data. So, only the recent data will be kept.


## Question 4
Review the AdventureWorks Schema [here](https://i.stack.imgur.com/LMu4W.gif)

Highlight at least two differences between it and your ERD. Would you change anything in yours?

The AdventureWorks Schema categorizes data into different schemas. In each schemas, it contains a set of tables. As for my ERD, data is not categorized. As the data set of my ERD is simple, categorization with schemas may not be needed. But if there is more information to be included in my ERD, I would categorize the data into Human Resources (include Emplyee, Employee shift etc.), Sales (include Sales, Custmer, Order etc.), Inventory (include book entitles etc.), and ther categories related to the book administration

Another difference is that the AdventureWrks Schema include ‘Modified Date’ at almost all table. This allow historical entry to be kept instead of being overwritten by updated data entry. My ERD utilize type 1 slowly changing dimension architecture. So, whenever I modify the book information using the same book_id, or modifying the customer information using the same customer_id, the previous data will be deleted and replaced by new data. In order to allow historical data to be kept, I may assign each edition of data an ID and ensure each entry will be record with the ‘Modified Date’. 

# Criteria

[Assignment Rubric](./assignment_rubric.md)

# Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `June 1, 2024`
* The branch name for your repo should be: `model-design`
* What to submit for this assignment:
    * This markdown (design_a_logical_model.md) should be populated.
    * Two Entity-Relationship Diagrams (preferably in a pdf, jpeg, png format).
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sql/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [V ] Create a branch called `model-design`.
- [V ] Ensure that the repository is public.
- [V ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [V ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
