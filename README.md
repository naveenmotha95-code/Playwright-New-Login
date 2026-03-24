*new*AdminPage.ts**
import { Page, Locator, expect } from '@playwright/test';
export default class AdminPage {
 readonly page: Page;
 private AdminLink: Locator;
 private EmpStatusEditbutton: Locator;
 private fillName: Locator;
 private saveBtn: Locator;
 private Addbtn: Locator;
 private sortAsc: Locator;
 private upgradeButton: Locator;
 private admindropdown: Locator;
 private adminSearch: Locator;
 private searchbutton: Locator;
 private userRoleElements: Locator;
 private username: Locator;
 constructor(page: Page) {
   this.page = page;
   this.AdminLink = page.locator('a[href*="admin/viewAdminModule"]');
   this.EmpStatusEditbutton = page.locator('.oxd-table-row .oxd-icon-button').nth(0);
   this.fillName = page.locator('input[placeholder="Type here"]');
   this.saveBtn = page.locator('button[type="submit"]');
   this.Addbtn = page.locator('button:has-text("Add")');
   this.sortAsc = page.locator('.oxd-table-sort');
   this.upgradeButton = page.locator('a:has-text("Upgrade")');
   this.admindropdown = page.locator('.oxd-select-text').first();
   this.adminSearch = page.locator('input[placeholder="Username"]');
   this.searchbutton = page.locator('button[type="submit"]');
   this.userRoleElements = page.locator('.oxd-select-option');
   this.username = page.locator('input[placeholder="Username"]');
 }
 async openAdminPage() {
   await this.AdminLink.click();
   await expect(this.page).toHaveURL(/admin/);
 }
 async EditEmpStatus(name: string) {
   await this.EmpStatusEditbutton.click();
   await this.fillName.fill(name);
   await this.saveBtn.click();
   await expect(this.page.locator('.oxd-table-row')).toContainText(name);
 }
 async AddEmpStatus(name: string) {
   await this.Addbtn.click();
   await this.fillName.fill(name);
   await this.saveBtn.click();
   await expect(this.page.locator('.oxd-table-row')).toContainText(name);
 }
 async upgrade() {
   const [newTab] = await Promise.all([
     this.page.context().waitForEvent('page'),
     this.upgradeButton.click()
   ]);
   await expect(newTab).toHaveURL(/upgrade/);
 }
 async adminRoles(username: string) {
   await this.admindropdown.click();
   await this.userRoleElements.nth(1).click(); // Admin
   await this.adminSearch.fill(username);
   await this.searchbutton.click();
   await expect(this.page.locator('.oxd-table-row')).toContainText(username);
 }
}
**BuzzPage.ts**
import { Page, Locator, expect } from '@playwright/test';
import path from 'path';
const filePath = path.resolve(__dirname, '../TestImage.jpg');
export default class BuzzPage {
 readonly page: Page;
 private buzzLink: Locator;
 private sharephoto: Locator;
 private buzzImage: Locator;
 private submitButton: Locator;
 private successMessage: Locator;
 private photoComment: Locator;
 private likeButton: Locator;
 private commentInput: Locator;
 private firstCommentButton: Locator;
 private latestComment: Locator;
 private editToggle: Locator;
 private postEdit: Locator;
 private postButton: Locator;
 constructor(page: Page) {
   this.page = page;
   this.buzzLink = page.locator('a[href*="buzz/viewBuzz"]');
   this.sharephoto = page.locator('button:has-text("Share Photos")');
   this.buzzImage = page.locator('input[type="file"]');
   this.submitButton = page.locator('button[type="submit"]');
   this.successMessage = page.locator('.oxd-toast-content');
   this.photoComment = page.locator('textarea[placeholder="Write your comment..."]');
   this.likeButton = page.locator('button:has-text("Like")').first();
   this.commentInput = page.locator('textarea').first();
   this.firstCommentButton = page.locator('button:has-text("Comment")').first();
   this.latestComment = page.locator('.oxd-comment').first();
   this.editToggle = page.locator('.oxd-buzz-post .oxd-icon-button').first();
   this.postEdit = page.locator('textarea[placeholder="Update your post..."]');
   this.postButton = page.locator('button:has-text("Save")');
 }
 async openBuzzPage() {
   await this.buzzLink.click();
   await expect(this.page).toHaveURL(/buzz/);
 }
 async sharePhotoPost(editCmnt: string) {
   await this.sharephoto.click();
   await this.buzzImage.setInputFiles(filePath);
   await this.photoComment.fill(editCmnt);
   await this.submitButton.click();
   await expect(this.successMessage).toContainText('Successfully');
 }
 async likeFirstPost() {
   await this.likeButton.click();
 }
 async addComment(comment: string) {
   await this.commentInput.fill(comment);
   await this.firstCommentButton.click();
   await expect(this.latestComment).toContainText(comment);
 }
 async editFirstPost(updatedText: string) {
   await this.editToggle.click();
   await this.postEdit.fill(updatedText);
   await this.postButton.click();
   await expect(this.page.locator('.oxd-buzz-post').first())
     .toContainText(updatedText);
 }
}
**LoginPage.ts**
import { Page, Locator, expect } from '@playwright/test';
const data = require('../Data/login.json');
export class LoginPage {
 readonly page: Page;
 private usernameInput: Locator;
 private passwordInput: Locator;
 private loginButton: Locator;
 private loginErrorMessage: Locator;
 private admin: Locator;
 private logout: Locator;
 constructor(page: Page) {
   this.page = page;
   this.usernameInput = page.locator('input[name="username"]');
   this.passwordInput = page.locator('input[name="password"]');
   this.loginButton = page.locator('button[type="submit"]');
   this.loginErrorMessage = page.locator('.oxd-alert-content-text');
   this.admin = page.locator('a[href*="admin/viewAdminModule"]');
   this.logout = page.locator('a:has-text("Logout")');
 }
 async openLoginPage() {
   await this.page.goto('https://opensource-demo.orangehrmlive.com/');
 }
 async PerformLogin() {
   await this.usernameInput.fill(data.username);
   await this.passwordInput.fill(data.password);
   await this.loginButton.click();
   await expect(this.page).toHaveURL(/dashboard/);
 }
 async loginWithCredentials(username: string, password: string) {
   await this.usernameInput.fill(username);
   await this.passwordInput.fill(password);
   await this.loginButton.click();
 }
 async verifyErrorMessage(errorMsg: string) {
   await expect(this.loginErrorMessage).toContainText(errorMsg);
 }
 async logoutUser() {
   await this.admin.click();
   await this.logout.click();
   await expect(this.page).toHaveURL(/auth/);
 }
}
 **MyInfoPage.ts**
import { Page, Locator, expect } from '@playwright/test';
import path from 'path';
const filePath = path.resolve(__dirname, '../TestImage.jpg');
export class MyInfoPage {
 readonly page: Page;
 private myInfo: Locator;
 private clickImage: Locator;
 private uploadButton: Locator;
 private fileInput: Locator;
 private imageSave: Locator;
 private helpbutton: Locator;
 constructor(page: Page) {
   this.page = page;
   this.myInfo = page.locator('a[href*="pim/viewMyDetails"]');
   this.clickImage = page.locator('.employee-image-wrapper img');
   this.uploadButton = page.locator('button:has-text("Upload")');
   this.fileInput = page.locator('input[type="file"]');
   this.imageSave = page.locator('button:has-text("Save")');
   this.helpbutton = page.locator('.oxd-icon.bi-question-lg');
 }
 async myInfoPage() {
   await this.myInfo.click();
   await expect(this.page).toHaveURL(/pim/);
   await this.clickImage.click();
   await this.fileInput.setInputFiles(filePath);
   await this.uploadButton.click();
   await this.imageSave.click();
   await expect(this.page.locator('.oxd-toast-content'))
     .toContainText('Successfully');
 }
 async hoverHelp() {
   await this.helpbutton.hover();
 }
}
