# Derrick Teo - Project 
## About the project
My team of 4 software engineering students and I were tasked with enhancing a basic command line interface desktop addressbook application for our Software Engineering project.
We chose to morph it into a travel management application called **TravelPal**. This application allows travellers to micromanage their travels, including features like expenditure and inventory managing.

project image

My role was to design thte __itinerary__ feature. The following sections illustrate these enhancement in more detail, as well as relevant documentation I have added to the user and developer guides in relation to these enhancements.

symbol legend

## Summary of contributions
This section shows a summary of my coding, documentation, and other helpful contributions to the team project. Our application is a mutipage application and features were split by assigning each individual a page to develop.

**Enhancement added:**

1. `Itinerary` Page
* What it does: The `Itinerary` Page is the main overview of user's trip. It contains basic information about the trip and provides shortcuts to each day in a trip.
* Justification: This is the landing page of their trip which gives the user the opportunity to navigate to each page. Also remind user of important information about the trip.
* Highlights: This enhancement is necessary to provide access to other pages.
2. `Days` Page
* What it does: It shows the list of days for the user to interact with
* Justification: This allows the user to have a overview of each day and its basic information. Commands on this page includes creating/editing/deleting days.
2. `Events` Page
* What it does: The `Events` Page provides a chronological overview of the events in each day. It also comprises a information panel which displays the information for each event. Commands on this page includes ceating/editing/deleting/showing details, of each event.
* Justification: User's want to know useful information for each event e.g. Inventory/Budget
3. `Edit Trip/Day/Event` Pages
* What it does: These pages are dynamic forms which update based on the user input. It processess the form to create/update each component.
* Justification: A neccessary feature to allow users to see their progress in updating a ther `Trip/Day/Event`.
* Highlights: Form items validate and upon update to each field. Fields accept several different data types e.g. Dates/Photo
4. `Trip` image feature
* What is does: Images can be added to each trip.
* Justification: Photos are important part of trips especially holidays.
5. `Navigation Bar` feature
* What is does: Provide navigability from one `Page` to another.
* Justification: Useful quality of life feature so users do not have to remember which pages have access to what other pages.

**Code contributed**: Please click these links to see a sample of my code:

**Other contributions:**

* Project management:
..* There were a total of 5 releases
* Enhancement to existing features: 
..* Responsible for maintaining the model component for `Trip/Day/Event` for other members to utilise and extend
..* Created `Page` for other members to develop features

## Contributions ot the User Guide

## Contributions to the Developer Guide

### [Itinerary] Edit trip/day/event feature
#### Implementation
Editing of trip/day/event can be accessed from `TripsPage/DaysPage/EventsPage` respectively. 
The execution of commands in the each page is facilitated by `TripManagerParser/DayViewParser/EventViewParser` and respectively.
They extend from `PageParser` class which serves as the abstraction for all parsers related to each __Page__.

The operations are exposed to the `Model` interface through the `Model#getPageStatus()`
method that returns the `PageStatus` containing the current state of application.

Given below is an example usage scenario and how the program behaves at each step.

Step 1. When the user launches the application. The `PageStatus` is initialized under along with other `Model` components. `PageStatus` at launch does not contain any `EditTripDescriptor/EditDayDescriptor/EditEventDescriptor` responsible for storing information for the edit.

image::ItineraryEdit0.png[]

Step 2. The user currently on the `TripsPage/DaysPage/EventsPage` is displayed a list of `Trip/Day/Event` respectively. The user executes the edit command `EDIT1` using the `OneBasedIndex` on the list to edit it.This executes the `EnterEditTripFieldCommand/EnterEditDayFieldCommand/EnterEditEventFieldCommand` that initializes a new descriptor within `PageStatus` before switching over to the `EditTripPage/EditDayPage/EditEventPage` containing to perform the editing.

image::ItineraryEdit1.png[]

Step 3. The user is now on the edit page displaying a list of fields that the user can edit in the `Trip/Day/Event`.
The following is an example list of commands available in `DaysPage` and the execution of the program when a field is edited in `DaysPage`:

* `edit n/<name> ds/<startDate> de/<endDate> b/<totalBudget> l/<destination> d/<description>` - Edits the relevant fields
* `done` - Completes the edit and returns to the __Overall View__
* `cancel` - Discards the edit and returns to the __Overall View__

The user executes the command `edit n/EditedName` on the `DaysPage`. The command creates a new descriptor from the contents of the original, replacing the fields only if they are edited. The new descriptor is then assigned to `PageStatus` replacing the original `EditDayDescriptor`. The result of the edit is then displayed to the user.

image::ItineraryEdit2.png[]

Step 4. The user has completed editing the `Trip/Day/Event` and executes `done`/`cancel` to confirm/discard the edit. The execution of the two cases are as follows:

* The user executes `done` to confirm the edit. This executes the `DoneEditTripCommand/DoneEditDayCommand/DoneEditEventCommand` and a `Trip/Day/Event` is built from the descriptor respective to the type it describes. `DayList#set(target, edited)` proceeds to be executed which accesses the `Day` to edit from the `day` field in `PageStatus` as the target. The method replaces the original day with the newly built day from the descriptor. The descriptor in `PageStatus` is then reset to contain empty fields.

image::ItineraryEdit3.png[]

* The User executes `cancel` to discard the edit. This executes the `CancelEditTripCommand/CancelEditDayCommand/CancelEditEventCommand` which resets the descriptor in `PageStatus` to contain all empty fields.

image::ItineraryEdit4.png[]

Upon completion of the edit, the user is returned to the `TripPage/DaysPage/EventsPage` depending on where the user entered the edit page from.

Below is an sequence diagram illustrating the execution of the command "edit ds/10/10/2019":

image::ItineraryEditSequenceDiagram.png[]

