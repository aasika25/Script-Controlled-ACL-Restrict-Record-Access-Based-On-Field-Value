# Script-Controlled ACL -- Restrict Record Access Based on Field Value

## Description

This lab demonstrates how to implement a script-controlled Access
Control List (ACL) to restrict users from viewing records unless they
meet specific conditions.

In this scenario, only users belonging to the EEE branch are allowed to
view records where the Branch field is set to EEE, while administrators
retain full access.

**Timer:** 3 hrs

## Milestone 1: Creation of Users and Roles

### User Creation

1.  Log in to the ServiceNow instance with admin access.
2.  Navigate to **User Administration → Users**.
3.  Click **New**.
4.  Create a test user with:
    -   **User ID:** EEE User
    -   **First name:** EEE
    -   **Last name:** User
    -   **Email:** eeeuser@gmail.com
5.  Save the record.

### Role Creation

1.  Navigate to **User Administration → Roles**.
2.  Click **New**.
3.  Enter the role name as `bb1`.
4.  Add a brief role purpose in the description.
5.  Click **Submit**.
6.  Create the other roles using the same procedure:
    -   `bb2`
    -   `bb3`
    -   `bb4`
7.  Assign all the roles to the EEE User.

## Tables Creation

1.  In the Application Navigator, search for **System Definition →
    Tables**.
2.  Click **New** and create a custom table with:
    -   **Label:** Institution Details
    -   **Name:** `u_institution_details`
    -   **Extends for:** false
3.  Click **Submit**.
4.  Add the following fields to the `u_institution_details` table:

  Field                 Type
  --------------------- ------------------------
  Student Roll Number   Auto Number
  Student Name          Reference - User
  Faculty Name          Reference - User
  Branch                Choice (ECE, EEE, CSE)
  Email                 String
  Phone Number          String
  Description           Multi String

5.  Save the table.
6.  Create multiple records in the Student Records table with different
    branch values such as **ECE, EEE, and CSE**.

## Creation of Access Control Lists (Read ACL)

1.  In the Application Navigator, search for **System Security → Access
    Control (ACL)**.
2.  Elevate the `Security_admin` role.
3.  Click **New** to create a new ACL.
4.  Enter:
    -   **Type:** record
    -   **Operation:** read
    -   **Name:** `u_institution_details`
    -   **Active:** true
    -   **Advanced:** true
5.  In the **Requires role** related list, add:
    -   `bb1`
6.  Add the Data condition:
    -   **Branch is EEE**
7.  Add the following script in the **Script** field:

``` javascript
(function () {
  // Allow admin users full access
  if (gs.hasRole('admin')) {
    return true;
  }

  // Allow only EEE branch users to see EEE records
  if (gs.hasRole('bb1')){
    return true;
  }

  // Deny access for all others
  return false;
})();
```

8.  Click **Submit** to save the ACL.

### Read ACL Verification

1.  Impersonate EEE users and open the Student Records list.
2.  Copy the table name `u_institution_details.list/LIST` and paste it
    in the All menu.
3.  Impersonate Admin and open the Student Records list.

**Verify:** - User with `bb1` role can view only EEE branch records. -
User without the role cannot view any records. - Admin user can view all
records regardless of branch.

## Creation of Access Control Lists (Create ACL)

1.  Click **New** to create a Create ACL.
2.  Enter:
    -   **Type:** record
    -   **Operation:** create
    -   **Name:** `u_institution_details`
    -   **Active:** true
3.  In the **Requires role** related list, add:
    -   `bb2`
4.  No Data condition is required.
5.  Save the ACL record.

### Create ACL Verification

-   User with `bb1` and `bb2` roles can view only EEE branch records and
    the **New** button.

## Creation of Access Control Lists (Write ACL)

1.  Click **New** to create a Write ACL.
2.  Enter:
    -   **Type:** record
    -   **Operation:** Write
    -   **Name:** `u_institution_details`
    -   **Active:** true
3.  In the **Requires role** related list, add:
    -   `bb3`
4.  No Data condition is required.
5.  Save the ACL record.

### Write ACL Verification

-   User with `bb1`, `bb2`, and `bb3` roles can view and edit the EEE
    branch records along with the **New** button.

## Creation of Access Control Lists (Delete ACL)

1.  Click **New** to create a Delete ACL.
2.  Enter:
    -   **Type:** record
    -   **Operation:** Delete
    -   **Name:** `u_institution_details`
    -   **Active:** true
3.  In the **Requires role** related list, add:
    -   `bb4`
4.  No Data condition is required.
5.  Save the ACL record.

### Delete ACL Verification

-   Users with `bb1`, `bb2`, `bb3`, and `bb4` roles can see, create,
    edit, and delete the EEE branch records.

## Outcome

By completing this lab, learners gain a comprehensive understanding of
how script-controlled **READ, WRITE, CREATE, and DELETE ACLs** enforce
record-level security, how user roles and record field values are
evaluated together to make access decisions, and how ACLs protect
sensitive data from unauthorized viewing, modification, creation, and
deletion across forms, lists, and Service Portal interfaces.

## Reference

This README was created from the provided **Script-Controlled ACL --
Micro Projects** PDF. fileciteturn0file0L2-L8
