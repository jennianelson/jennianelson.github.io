---
layout: post
title:      "Flatiron Rails Project: Progress Tracker"
date:       2018-03-24 00:13:30 +0000
permalink:  flatiron_rails_project_progress_tracker
---

I told myself over and over at the beginning of this project to "keep it simple, stupid."  While I don't have trouble resisting the urge to go overboard with front-end styling, I am tantalized by features that seem just beyond my reach.  Like Icarus, I flew too high a few times, breaking everything and/or realizing that I would have to completely rethink my models and associations.  Because I pushed myself, however, I learned a lot, and I was able to clear up some concepts that had previously given me trouble (ahem, ActiveRecord queries).

To help me check that I've met all of Flatiron's requirements for this project, and that I can explain my code and Rails concepts clearly, I'm going to take you through the experience using the app as an administrator, as a teacher, and as a student.

## App Overview

As a former middle school English teacher, I often wished for a tool that would allow students to reflect on and record their progress through course standards. This would not only allow them to take more ownership of their learning, but also see their growth over time. Additionally, standards-based grading and assessment is replacing traditional grading in many schools, so this tool could be beneficial in helping students clearly understand the expecations in each of their courses. In order to populate course standards, I used the Common Standards Project API, a database of academic standards from all 50 states, organzations, districts, and schools. Click [here](https://github.com/commonstandardsproject/api) for more information about this amazing project. As I am just learning to code, and this this is my first Rails application, I decided to work with only one jurisdiction of standard sets to start--my home state of Alabama.  To meet the project requirements, I did not pre-load the standards into my database; rather, the admin role is responsible for creating subjects and standards from the API, making them available to students and teachers.  If I continued to develop this app for actual use, each school would have their own domain able to be managed by the technology coordinator, an administrator, or by individual teachers.

## Models
### Here it is in "hiero-Gliffy" (see what I did there?)
<a href="https://imgur.com/0Zszqtp"><img src="https://i.imgur.com/0Zszqtp.jpg" title="source: imgur.com" /></a>

### In other words...

#### A User
* has many subjects through student subjects
* has many standards through student standards
* has many student subjects
* has many student standards

#### A Subject
* hss many sections
* has many standards
* has many student subjects
* has many users through student subjects

#### A Section
* has many standards
* belongs to a subejct

#### A Standard
* belongs to a section
* belongs to a subject
* has many student standards

#### A Student Subject (Join Model)
* has many student standards
* belongs to a subject
* belongs to a user

#### A Student Standard (Join Model)
* belongs to a standard
* belongs to a user
* belongs to a student subject

## User Roles/Permissions:
I used the [Devise](https://github.com/plataformatec/devise) gem for authentication and [Pundit](https://github.com/varvet/pundit) policies for authorization.  I created three user roles using ActiveRecord's *[enum](http://api.rubyonrails.org/classes/ActiveRecord/Enum.html)* attribute.

### Admin:
* Responsible for creating, updating, and deleting subjects, sections, and standards from the app's database. *See note in next section about this*.
* Can delete student-owned subjects and standards.
* Can change a user's permissions or delete a user's account.

### Student:
* Can add subjects from the database to his/her personal homepage. This creates a unique copy of the subject and the standards for the student.
* Can choose a "Progress "Level" and add comments and learning evidence for each standard.

### Teacher:
* Can do whatever a student can but can also view the progress of individual students.


## User Experience

### Admin
*Note*: If I were to build this app for the "real world", I would preload the database with all of the different state, national, and organization-specific standards (these are called jurisdictions in the Common Standards Project).  The role of an admin would then just be to manage teacher and student accounts and the data that they create.  Because this project required that I use nested forms and resources, however, I decided to make the admin also responsible for adding subjects and standards to the database as needed.

#### Admin Homepage: Subject#Index
* This page lists all of the subjects that have been added to the database.  If I had discovered (and decided to use) the Common Standards Project earlier in my project, design, I would have included a grade attribute and pulled that in for easier filtering; alas, I did not, and because this feature isn't important to the main user--the students--I decided not to add it at this stage.
* The title of each subject links to that subject's show page; the dropdown menu links to each subject's page as well.
* This page also includes a button that goes to the subjects#new page.

<a href="https://imgur.com/4F0PO8T"><img src="https://i.imgur.com/4F0PO8T.png" title="source: imgur.com" /></a>

#### Subject#New

* The admin chooses a subject from the Common Standards Project API using the dropdown menu.  The form includes a hidden field that collects the subject's set id.  This is then used to get the standard set for that subject from the API.
* The admin also enters a title for the subject and each of its sections and clicks "Create Subject."
* This redirects to Subjects#Show.

<a href="https://imgur.com/sUh3Nwq"><img src="https://i.imgur.com/sUh3Nwq.png" title="source: imgur.com" /></a>

#### Subject#Show

* This page shows lists the subject's sections and also includes a form with a simple checkbox, allowing the admin to decide when the subject is ready for students and teachers to use.

* The admin can either add the subject's standards all at once on the subject#edit page or one section at a time on the section#edit page.

<a href="https://imgur.com/ckeWBDk"><img src="https://i.imgur.com/ckeWBDk.png" title="source: imgur.com" /></a>

#### Subject#Edit

* This page contains two forms: one to add, change, or delete a section, and another to add standards.  The reason that these forms are separate but both on this page is because the form to add standards uses the section titles to put each standard in the appropriate section.
* The admin will check the boxes next to each standard that should be added, select its section title, and if the API does not include a standard number (known as the statement notation), he or she will type in a number, designating a sub-standards by using a lowercase letter.
* This form also includes a hidden field that captures each standards "ASN ID" from the API.  This number is used to sort the standards.
* Clicking "Update Subject" will refresh the edit page and alert the user that the subject has been updated successfully.
* Clicking "Update Subject Standards" on the add standards form will redirect the user to the subject#show page.

<a href="https://imgur.com/ltnxpCN"><img src="https://i.imgur.com/ltnxpCN.png" title="source: imgur.com" /></a>

<a href="https://imgur.com/AACGB4q"><img src="https://i.imgur.com/AACGB4q.png" title="source: imgur.com" /></a>

#### Section#Edit

* If the admin would rather add standards in smaller batches, he or she will click on the "Add Standards to this Section" link next to each section's title.
* All that is needed to add a standard is to click its check box and add a number if one is not provided by the API.
* This form redirects to the subject#show page.

<a href="https://imgur.com/Fve89fS"><img src="https://i.imgur.com/Fve89fS.png" title="source: imgur.com" /></a>

#### Managing Users (User#Index)

* Another admin feature is the ability to delete a user's account or to change a user's role.  The default role for a new user is "student," so the admin would be responsible for deciding which accounts should have teacher or admin access.

<a href="https://imgur.com/f4Hwlno"><img src="https://i.imgur.com/f4Hwlno.png" title="source: imgur.com" /></a>

#### User#Show

* An admin can also delete student subjects (and all associated student standards) from the User#Show page.

IMAGE OF USERSHOW

### Students

#### Homepage: StudentSubjects#Index

* StudentSubject is the join model that allows a subject to have many students, and a student to have many subjects.
* The homepage for students contains a dropdown menu that includes all of the subjects the admin has added to the database and marked as "ready" for students to access.
* When a student chooses a subject and clicks "Add Subject", a student subject is created for them and added to their homepage.  
* Adding a subject also creates a "student standard" for each standard associated with that subject.  The StudentStandard join model allows students to have many standards and a standards to have many students.  Each student standard also has writable "comments" and "evidence" attributes which will be explained shortly.
* When a student adds a new subject or clicks on one already listed on their homepage, they are taken to the studentsubject#show page.

<a href="https://imgur.com/KthFVYi"><img src="https://i.imgur.com/KthFVYi.png" title="source: imgur.com" /></a>

#### StudentSubject#Show

* This page lists each section of that subject and also gives students the option to view their standards by progress level.

<a href="https://imgur.com/oil8QLx"><img src="https://i.imgur.com/oil8QLx.png" title="source: imgur.com" /></a>

* When a student clicks on a section's title, they are taken to the section#show page which is nested within the subject.  This page displays each standard and progress level in a table and a link to add or view the their progress.

<a href="https://imgur.com/SgelDiW"><img src="https://i.imgur.com/SgelDiW.png" title="source: imgur.com" /></a>

* The "Add Progress" link takes the student to the StudentStandard#Edit page.
* This is where the student can choose their progress level (beginning, progressing, confused, conquered), enter comments, and list any assignments or assessments that demonstrate what they've learned.

<a href="https://imgur.com/twwkrGo"><img src="https://i.imgur.com/twwkrGo.png" title="source: imgur.com" /></a>

* Students can also view the progress they've entered on the StudentStandard#Show page.

<a href="https://imgur.com/twwkrGo"><img src="https://i.imgur.com/twwkrGo.png" title="source: imgur.com" /></a>

 ### Teachers
 
* Teachers have access to everything a student does, allowing them to demonstrate to their students how to use the app and see exactly what students are seeing. 
* Teachers also have the ability to view a student's progress level, comments, and evidence but not to edit a student's standards.
* Teachers access student pages by clicking on the "Students" tab at the top of the page.  This is the User#index page filtered to only show student users.

<a href="https://imgur.com/4B9VDvQ"><img src="https://i.imgur.com/4B9VDvQ.png" title="source: imgur.com" /></a>

* Clicking on a student's email address goes to the user's show page.  Teachers are able to then click on a section to view student progress or click on the subject to view the student's progress by progress level.

<a href="https://imgur.com/qIFwHek"><img src="https://i.imgur.com/qIFwHek.png" title="source: imgur.com" /></a>

* Notice that the teacher's view of these pages is different than that of the student's.

<a href="https://imgur.com/sM2OJEi"><img src="https://i.imgur.com/sM2OJEi.png" title="source: imgur.com" /></a>

### And that's it!

I would like to add that I got my first job as a developer with this app, a snazzy-looking, yet content-slim resume I made on Canva, a creative cover letter, and the relentless pushing of a colleague at the YMCA.  Keep making progress, y'all!

