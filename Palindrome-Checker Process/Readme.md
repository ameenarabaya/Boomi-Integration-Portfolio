# Palindrome-Checker Process

## Description
This Boomi integration validates whether a string read from a local file
is a palindrome. The process includes error handling, data mapping, and
email notification.

## Integration Flow
- Starts with a Try/Catch block for error handling
- Reads a word from a local file using Disk v2 connector
- Checks whether the string is a palindrome using a Map shape
- Sets dynamic document properties with the validation result
- Sends an email notification with the result
- Ends the process successfully or handles errors in the Catch flow

## Key Concepts Used
- Try/Catch Error Handling
- Disk v2 Connector
- XML Processing
- Data Mapping
- Dynamic Document Properties
- Email Notification

## Tools & Technologies
- Boomi AtomSphere
- Disk v2 Connector
- XML Profiles
- Mail Connector

## Screenshots
Screenshots illustrate the process flow, palindrome validation logic.

