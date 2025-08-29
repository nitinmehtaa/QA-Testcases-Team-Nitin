# Test Case: TC_User_Profile_Update
**Story ID:** 1238
**Feature:** User Profile Management
**Priority:** Medium
**Linked Bug IDs:** 5682
**Owner:** Nitin Mehta
**State:** Manual
**Sprint:** S-01
**Regression:** Yes

**Preconditions:**
1. User is logged into the system
2. User profile section is accessible
3. Image upload functionality is enabled
4. Valid test data is prepared for profile updates

**Test Steps:**
1. Navigate to user profile section
2. Update personal information:
   a. Full name
   b. Contact number
   c. Address details
   d. Date of birth
3. Upload new profile picture:
   a. Select image file (JPG/PNG)
   b. Crop/adjust image if needed
   c. Save changes
4. Modify email preferences:
   a. Newsletter subscription
   b. Notification settings
   c. Communication frequency
5. Save all profile changes
6. Logout and log back in
7. Verify all updated information persists

**Expected Results:**
1. All profile fields accept valid inputs
2. Profile picture uploads successfully
3. Email preference changes are saved
4. Success message displayed after updates
5. All changes persist after re-login
6. Profile data is consistent across all pages