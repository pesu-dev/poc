# Capstone Finder

> [!WARNING]
> This is a work in progress. Do not merge this to the main branch.

## Overview

This project aims to help students find members for their capstone projects. Students can search for members by branch, gender and GPA, three of the criteria that are required for diversity in capstone groups.

---

## Features

- Search for members by branch, gender and GPA
- Add resume to your profile
- View member profiles
- Chat with members before giving out contact information
- Auto delete chats after 1 week
- Auto delete account after 6 months of inactivity

---

## Basic Tech Stack

- Next.js
- Tailwind CSS
- Shadcn UI
- MongoDB as database
- `jose` for JWT
- `pesu-auth` for authentication

---

## Workflows

### Student Signup

- Student uses PESUAcademy credentials to sign in to `pesu-auth`
- `pesu-auth` fetches details for the student, and saves **SRN**, **name**, **branch**, **campus**, **email** and **phone** information
- Email and phone are encrypted and saved in the database
- Student then enters CGPA, gender and bio
- Student can also optionally add social links like GitHub and LinkedIn for others to reach out
- Student now gets a profile link to share with people
- Student can enable and disable the "searching" switch, which hides profile when off

### Searching

- Another student signs up and visits "Find" page
- Student selects campus, gender, branch and GPA range to filter students
- Student can optionally search for someone with name
- Once a suitable member is found, student can either save their profile to visit later, or send a request for contact details
- The student gets an email to either approve or share their contact details

### Moderation

- Admin accounts exist, which have complete control over all accounts, but cannot view chats
- Students can report chats, which can be seen by admins and the target student's account may be suspended or banned
- Admins can also take down accounts based on profile data

### Cron jobs

- Biannual cron job: Deletes inactive accounts (based on last login time)

---

## Add-ons

- Simple end-to-end encryption for student chats
- Resume uploading
