*nneewe*AdminPage.ts**
import { Page, Locator } from '@playwright/test';

export default class AdminPage {
  readonly page: Page;

  private adminLink: Locator;
  private editBtn: Locator;
  private input: Locator;
  private saveBtn: Locator;
  private addBtn: Locator;
  private upgradeBtn: Locator;
  private configTab: Locator;
  private modulesTab: Locator;

  constructor(page: Page) {
    this.page = page;

    this.adminLink = page.locator('a[href*="admin/viewAdminModule"]');
    this.editBtn = page.locator('.oxd-icon-button').nth(0);
    this.input = page.locator('input[placeholder="Type here"]');
    this.saveBtn = page.locator('button[type="submit"]');
    this.addBtn = page.locator('button:has-text("Add")');
    this.upgradeBtn = page.locator('a:has-text("Upgrade")');
    this.configTab = page.locator('a:has-text("Configuration")');
    this.modulesTab = page.locator('a:has-text("Modules")');
  }

  async EditEmpStatus(name: string) {
    await this.adminLink.click();
    await this.page.waitForLoadState('networkidle');

    await this.editBtn.click();
    await this.input.fill(name);
    await this.saveBtn.click();
  }

  async AddEmpStatus(name: string) {
    await this.adminLink.click();
    await this.page.waitForLoadState('networkidle');

    await this.addBtn.click();
    await this.input.fill(name);
    await this.saveBtn.click();
  }

  async upgrade() {
    await this.adminLink.click();
    await this.upgradeBtn.click();
  }

  async adminModules() {
    await this.adminLink.click();
    await this.configTab.click();
    await this.modulesTab.click();
  }
}
**BuzzPage.ts**
import { Page, Locator } from '@playwright/test';
import path from 'path';

const filePath = path.resolve(__dirname, '../TestImage.jpg');

export default class BuzzPage {
  readonly page: Page;

  private buzzLink: Locator;
  private shareBtn: Locator;
  private fileInput: Locator;
  private commentBox: Locator;
  private postBtn: Locator;

  private mostCommentTab: Locator;
  private commentInput: Locator;
  private commentBtn: Locator;

  private editToggle: Locator;
  private editText: Locator;
  private saveBtn: Locator;

  constructor(page: Page) {
    this.page = page;

    this.buzzLink = page.locator('a[href*="buzz/viewBuzz"]');
    this.shareBtn = page.locator('button:has-text("Share Photos")');
    this.fileInput = page.locator('input[type="file"]');
    this.commentBox = page.locator('textarea[placeholder="Write your comment..."]');
    this.postBtn = page.locator('button[type="submit"]');

    this.mostCommentTab = page.locator('button:has-text("Most Commented")');

    this.commentInput = page.locator('textarea').first();
    this.commentBtn = page.locator('button:has-text("Comment")').first();

    this.editToggle = page.locator('.oxd-buzz-post .oxd-icon-button').first();
    this.editText = page.locator('textarea[placeholder="Update your post..."]');
    this.saveBtn = page.locator('button:has-text("Save")');
  }

  async sharePhotoPost(comment: string) {
    await this.buzzLink.click();
    await this.page.waitForLoadState('networkidle');

    await this.shareBtn.click();
    await this.fileInput.setInputFiles(filePath);
    await this.commentBox.fill(comment);
    await this.postBtn.click();
  }

  async mostcommentTab() {
    await this.buzzLink.click();
    await this.mostCommentTab.click();
  }

  async addCommentToPost(comment: string) {
    await this.buzzLink.click();

    await this.commentInput.fill(comment);
    await this.commentBtn.click();

    await this.page.waitForSelector('.oxd-comment');
  }

  async editPost(comment: string, editPostText: string) {
    await this.buzzLink.click();

    await this.shareBtn.click();
    await this.fileInput.setInputFiles(filePath);
    await this.commentBox.fill(comment);
    await this.postBtn.click();

    await this.editToggle.click();
    await this.editText.fill(editPostText);
    await this.saveBtn.click();
  }
}
**LoginPage.ts**
import { Page, Locator, expect } from '@playwright/test';
const data = require('../Data/login.json');

export class LoginPage {
  readonly page: Page;

  private username: Locator;
  private password: Locator;
  private loginBtn: Locator;

  constructor(page: Page) {
    this.page = page;

    this.username = page.locator('input[name="username"]');
    this.password = page.locator('input[name="password"]');
    this.loginBtn = page.locator('button[type="submit"]');
  }

  async performLogin() {
    await this.page.goto('https://opensource-demo.orangehrmlive.com/');

    await this.username.fill(data.username);
    await this.password.fill(data.password);
    await this.loginBtn.click();

    await expect(this.page).toHaveURL(/dashboard/);
  }
}
 **MyInfoPage.ts**
import { Page, Locator } from '@playwright/test';
import path from 'path';

const filePath = path.resolve(__dirname, '../TestImage.jpg');

export class MyInfoPage {
  readonly page: Page;

  private myInfo: Locator;
  private image: Locator;
  private uploadBtn: Locator;
  private fileInput: Locator;
  private saveBtn: Locator;
  private helpBtn: Locator;

  constructor(page: Page) {
    this.page = page;

    this.myInfo = page.locator('a[href*="pim/viewMyDetails"]');
    this.image = page.locator('.employee-image-wrapper img');
    this.uploadBtn = page.locator('button:has-text("Upload")');
    this.fileInput = page.locator('input[type="file"]');
    this.saveBtn = page.locator('button:has-text("Save")');
    this.helpBtn = page.locator('.oxd-icon.bi-question-lg');
  }

  async myInfoPage() {
    await this.myInfo.click();
    await this.image.click();
    await this.fileInput.setInputFiles(filePath);
    await this.uploadBtn.click();
    await this.saveBtn.click();

    await this.page.waitForSelector("//p[contains(@class,'toast-message')]");
  }

  async helpHover() {
    await this.myInfo.click();
    await this.helpBtn.hover();
  }
}
