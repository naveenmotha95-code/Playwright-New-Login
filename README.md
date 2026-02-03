**AdminPage.ts**
import { Page, Locator, expect } from "@playwright/test";
export default class AdminPage {
 readonly page: Page;
 private AdminLink: Locator;
 private EmpStatusEditbutton: Locator;
 private fillName: Locator;
 private saveBtn: Locator;
 private Addbtn: Locator;
 private sortAsc: Locator;
 private upgradeButton: Locator;
 private EmpStatus: Locator;
 private admindropdown: Locator;
 private adminSearch: Locator;
 private searchbutton: Locator;
 private userRoleElements: Locator;
 private username: Locator;
 private usernamelist: Locator;
 private configTab: Locator;
 private modulesubTab: Locator;
 private jobbtn: Locator;
 private jobcatbtn: Locator;
 constructor(page: Page) {
   this.page = page;
   // Menu Links
   this.AdminLink = page.locator('a[href="/web/index.php/admin/viewAdminModule"]');
   // Employee Status
   this.EmpStatusEditbutton = page.locator(".oxd-table-row .oxd-icon-button").nth(0);
   this.fillName = page.locator('input[placeholder="Type here ..."]');
   this.saveBtn = page.locator('button[type="submit"]');
   this.Addbtn = page.locator('button:has-text("Add")');
   // Sorting + Upgrade
   this.sortAsc = page.locator(".oxd-table-sort-asc");
   this.upgradeButton = page.locator('a:has-text("Upgrade")');
   // Role & Search
   this.admindropdown = page.locator(".oxd-select-text");
   this.adminSearch = page.locator('input[placeholder="Username"]');
   this.searchbutton = page.locator('button[type="submit"]');
   // User roles
   this.userRoleElements = page.locator(".oxd-select-dropdown .oxd-select-option");
   // Username fields
   this.username = page.locator('input[placeholder="Username"]');
   this.usernamelist = page.locator(".oxd-table-row");
   // Tabs
   this.configTab = page.locator('a:has-text("Configuration")');
   this.modulesubTab = page.locator('a:has-text("Modules")');
   // Job section
   this.jobbtn = page.locator('a:has-text("Job")');
   this.jobcatbtn = page.locator('a:has-text("Job Categories")');
 }
 // Navigate to Admin Page
 async openAdminPage() {
   await this.AdminLink.click();
   await expect(this.page).toHaveURL(/admin/);
 }
 // Edit Employment Status
 async EditEmpStatus(name: string) {
   await this.EmpStatusEditbutton.click();
   await this.fillName.fill(name);
   await this.saveBtn.click();
   await expect(this.page.locator(".oxd-table-row")).toContainText(name);
 }
 // Add a new Employment Status
 async AddEmpStatus(name: string) {
   await this.Addbtn.click();
   await this.fillName.fill(name);
   await this.saveBtn.click();
   await expect(this.page.locator(".oxd-table-row")).toContainText(name);
 }
 // Upgrade button method
 async upgrade() {
   const newTabPromise = this.page.context().waitForEvent("page");
   await this.upgradeButton.click();
   const newTab = await newTabPromise;
   await expect(newTab).toHaveURL(/upgrade/);
 }
 // Select Admin role and search
 async adminRoles(username: string) {
   await this.admindropdown.click();
   await this.userRoleElements.nth(1).click(); // selects "Admin"
   await this.adminSearch.fill(username);
   await this.searchbutton.click();
 }
}  

**BuzzPage.ts**
import { Page, Locator, expect } from "@playwright/test";
import path from "path";
// File path to upload image
const filePath = path.resolve(__dirname, "../../TestImage.jpg");
/**
* Page Object Model for Buzz Page interactions.
*/
export default class BuzzPage {
 readonly page: Page;
 private mostCmntTab: Locator;
 private buzzLink: Locator;
 private sharephoto: Locator;
 private buzzImage: Locator;
 private submitButton: Locator;
 private successMessage: Locator;
 private photoComment: Locator;
 private firstPostFooter: Locator;
 private likeButton: Locator;
 private likeCount: Locator;
 private commentInput: Locator;
 private firstCommentButton: Locator;
 private latestComment: Locator;
 private editToggle: Locator;
 private postEdit: Locator;
 private postButton: Locator;
 private verifyCmnt: Locator;
 private cmnt: Locator;
 constructor(page: Page) {
   this.page = page;
   // Tabs and Links
   this.mostCmntTab = page.locator('button:has-text("Most Commented")');
   this.buzzLink = page.locator('a[href="/web/index.php/buzz/viewBuzz"]');
   // Photo Upload
   this.sharephoto = page.locator('button:has-text("Share Photos")');
   this.buzzImage = page.locator('input[type="file"]');
   this.submitButton = page.locator('button[type="submit"]');
   this.successMessage = page.locator(".oxd-toast-content");
   // Post + Comments
   this.photoComment = page.locator('textarea[placeholder="Write your comment..."]');
   this.firstPostFooter = page.locator(".oxd-buzz-post .oxd-buzz-post-footer").first();
   this.likeButton = this.firstPostFooter.locator('button:has-text("Like")');
   this.likeCount = this.firstPostFooter.locator(".oxd-text").nth(1);
   this.commentInput = this.firstPostFooter.locator('textarea');
   this.firstCommentButton = this.firstPostFooter.locator('button:has-text("Comment")');
   this.latestComment = page.locator(".oxd-comment").first();
   // Edit Post
   this.editToggle = page.locator(".oxd-buzz-post .oxd-icon-button").first();
   this.postEdit = page.locator('textarea[placeholder="Update your post..."]');
   this.postButton = page.locator('button:has-text("Save")');
   // Verification Comment Locator
   this.verifyCmnt = page.locator(".oxd-comment").first();
   this.cmnt = page.locator(".oxd-comment").first();
 }
 // Navigate to Buzz Page
 async openBuzzPage() {
   await this.buzzLink.click();
   await expect(this.page).toHaveURL(/buzz/);
 }
 // Upload a photo & comment
 async sharePhotoPost(editCmnt: string) {
   await this.sharephoto.click();
   await this.buzzImage.setInputFiles(filePath);
   await this.photoComment.fill(editCmnt);
   await this.submitButton.click();
   await expect(this.successMessage).toContainText("Successfully");
 }
 // Like the first post
 async likeFirstPost() {
   await this.likeButton.click();
 }
 // Add a comment to the first post
 async addComment(comment: string) {
   await this.commentInput.fill(comment);
   await this.firstCommentButton.click();
   await expect(this.latestComment).toContainText(comment);
 }
 // Edit the first post
 async editFirstPost(updatedText: string) {
   await this.editToggle.click();
   await this.postEdit.fill(updatedText);
   await this.postButton.click();
   await expect(this.page.locator(".oxd-buzz-post").first()).toContainText(updatedText);
 }
}

**LoginPage.ts**
import { Locator, Page, expect } from "@playwright/test";
// Importing login test data
const data = JSON.parse(JSON.stringify(require("../Data/login.json")));
/**
* Page Object Model class for the Login Page
*/
export class LoginPage {
 readonly page: Page;
 // Locators for login fields and actions
 private usernameInput: Locator;      // Username input
 private passwordInput: Locator;      // Password input
 private loginButton: Locator;        // Login button
 private loginErrorMessage: Locator;  // Login error message
 private admin: Locator;              // Admin menu
 private logout: Locator;             // Logout option
 constructor(page: Page) {
   this.page = page;
   // Initialize all locators
   this.usernameInput = page.locator('input[name="username"]');
   this.passwordInput = page.locator('input[name="password"]');
   this.loginButton = page.locator('button[type="submit"]');
   this.loginErrorMessage = page.locator(".oxd-alert-content-text");
   // After login
   this.admin = page.locator('a[href="/web/index.php/admin/viewAdminModule"]');
   this.logout = page.locator('a:has-text("Logout")');
 }
 // Navigate to login URL
 async openLoginPage() {
   await this.page.goto("https://opensource-demo.orangehrmlive.com/");
 }
 // Perform login using JSON test data
 async PerformLogin() {
   await this.usernameInput.fill(data.username);
   await this.passwordInput.fill(data.password);
   await this.loginButton.click();
   // Expect dashboard URL after successful login
   await expect(this.page).toHaveURL(/dashboard/);
 }
 // Login using custom values (optional helper)
 async loginWithCredentials(username: string, password: string) {
   await this.usernameInput.fill(username);
   await this.passwordInput.fill(password);
   await this.loginButton.click();
 }
 // Verify login error message (for negative test)
 async verifyErrorMessage(errorMsg: string) {
   await expect(this.loginErrorMessage).toContainText(errorMsg);
 }
 // Logout
 async logoutUser() {
   await this.admin.click();
   await this.logout.click();
   await expect(this.page).toHaveURL(/auth/);
 }
}

 **MyInfoPage.ts**
import { Locator, Page, expect } from "@playwright/test";
import path from "path";
// Resolving the path to the image used for upload
const filePath = path.resolve(__dirname, "../../TestImage.jpg");
/**
* Page Object Model for the "My Info" section of the application.
*/
export class MyInfoPage {
 readonly page: Page;
 // Locators for elements in My Info section
 private myInfo: Locator;
 private clickImage: Locator;
 private uploadButton: Locator;
 private fileInput: Locator;
 private imageSave: Locator;
 private helpbutton: Locator;
 constructor(page: Page) {
   this.page = page;
   // Initialize locators
   this.myInfo = page.locator('a[href="/web/index.php/pim/viewMyDetails"]');   // My Info tab
   this.clickImage = page.locator(".employee-image-wrapper img");              // Profile image click
   this.uploadButton = page.locator('button:has-text("Upload")');              // Upload btn
   this.fileInput = page.locator('input[type="file"]');                        // File input
   this.imageSave = page.locator('button:has-text("Save")');                   // Save image button
   this.helpbutton = page.locator('i.oxd-icon.bi-question-lg');               // Help icon
 }
 /**
  * Navigates to My Info, uploads a profile image, and saves it.
  */
 async myInfoPage() {
   // Go to My Info tab
   await this.myInfo.click();
   await expect(this.page).toHaveURL(/pim/);
   // Click the profile image to open upload dialog
   await this.clickImage.click();
   // Upload the file
   await this.fileInput.setInputFiles(filePath);
   // Click upload button
   await this.uploadButton.click();
   // Click Save button
   await this.imageSave.click();
   // Wait for upload success toast
   await expect(this.page.locator(".oxd-toast-content")).toContainText("Successfully");
 }
 /**
  * Hover over help button to verify tooltip or action.
  */
 async hoverHelp() {
   await this.helpbutton.hover();
 }
}
