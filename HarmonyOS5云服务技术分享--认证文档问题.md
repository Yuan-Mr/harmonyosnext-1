Hello, developers! This article will elaborate on how to integrate Huawei AppGallery Connect (AGC) authentication services based on the HarmonyOS ArkTS framework, covering the entire process from project creation to SDK integration. Whether you are accessing AGC services for the first time or need to optimize existing processes, this article provides a complete guide.  


### I. Detailed Development Process  
1. **Create a Project and App**  
   - **Role**: A project serves as the organizational entity for AGC resources, supporting centralized management of multi-platform versions (e.g., mobile phones, tablets) of the same app.  
   - **Scenario Suggestions**:  
     - Distinguish between testing and production environments by creating different projects.  
     - Each project can independently manage authentication service configurations for different versions.  

2. **Enable Authentication Services**  
   Log in to the AGC console, go to the target project, and enable the required authentication methods (e.g., mobile phone, email, Huawei Account, etc.) on the *Build > Authentication Service* page.  

3. **Obtain the agconnect-services.json File**  
   - **Operation Path**: AGC Console → Project Settings → App Configuration → Download the configuration file.  
   - **Role**: This file contains the necessary keys and configuration information for the app to communicate with AGC services.  

4. **Integrate the SDK**  
   - **Core Dependencies**: AGC SDK + Authentication Service SDK.  
   - **Detailed Steps**:  
     - Configure HarmonyOS project dependencies (see the *SDK Integration* section below).  
     - Initialize the SDK and add network permissions.  

5. **Implement Account Login Authentication**  
   - **Supported Methods**:  
     - Standard login: Mobile phone, email, Huawei Account, self-owned account, anonymous account.  
   - **Advanced Features**:  
     - Account linking: Supports associating multiple account systems with the same user identity.  
     - Anonymous to real-name: Upgrade anonymous users to real-name accounts.  

6. **Logout**  
   - **Function Description**:  
     - Clear local user information and tokens.  
     - Applicable scenarios: User switches accounts or temporarily logs out.  

7. **Account Deletion**  
   - **Security Requirements**:  
     - Users must initiate deletion actively to ensure compliance with privacy regulations.  
     - After deletion, user data on the AGC side will be permanently deleted.  


### II. Full Process of SDK Integration  
#### Prerequisites  
- **Development Tools**: DevEco Studio 5.0.3.100+  
- **SDK Version**:  
  - Compile SDK Version ≥ 12  
  - Compatible SDK Version ≥ 12  

1. **Add the App Configuration File**  
   Copy agconnect-services.json to the project directory:  
   `AppScope/resources/rawfile/`  
   - **Note**: If the rawfile directory does not exist, create it manually.  

2. **Configure SDK Dependencies**  
   - **Method 1: Through oh-package.json5**  
     Add dependencies in the app-level oh-package.json5:  
     ```json  
     "dependencies": {  
       "@hw-agconnect/auth": "^1.0.4"  
     }  
     ```  
     Click *Sync Now* in the upper right corner to synchronize the configuration.  
   - **Method 2: Command Line Installation**  
     Enter the entry directory and execute the command:  
     `ohpm install @hw-agconnect/auth`  

3. **Initialize the SDK**  
   Initialize in the onCreate of EntryAbility.ets:  
   ```typescript  
   import auth from '@hw-agconnect/auth';  

   onCreate(want, launchParam) {  
     // Read the configuration file  
     let file = this.context.resourceManager.getRawFileContentSync('agconnect-services.json');  
     let json: string = buffer.from(file.buffer).toString();  
     // Initialize the AGC SDK  
     auth.init(this.context, json);  
   }  
   ```  
   - **Add Network Permissions**: Declare in module.json5:  
     ```json  
     "requestPermissions": [  
       { "name": "ohos.permission.INTERNET" }  
     ]  
     ```  

4. **Manually Set Client ID/Secret (Optional)**  
   - **Applicable Scenarios**: When the configuration file does not contain keys (when downloading, check *Do not include keys*).  
   - **Operation Steps**:  
     - Obtain the Client ID and Secret in *Project Settings > General* of the AGC console.  
     - Supplement parameters after initialization:  
       ```typescript  
       auth.setClientId("xxx");  // Replace with the actual value  
       auth.setClientSecret("xxx");  
       ```  

5. **Configure Obfuscation Rules**  
   - **Rules File**: entry/obfuscation-rules.txt  
   - **Add Content**:  
     ```  
     -keep  
     XXX/oh_modules/@hw-agconnect/auth  
     ```  
     - **Path Description**: XXX is the actual path of the SDK in oh_modules (e.g., entry/oh_modules).  


### III. Conclusion  
Through this article, you have completed the process of integrating AGC authentication services with HarmonyOS ArkTS. In the future, you can expand login methods (such as third-party social accounts) based on business needs and monitor user behavior data through the AGC console. If you encounter issues in practice, please visit the Huawei Developer Forum or AGC official documentation for technical support.  

If there are other features you want to learn about, feel free to leave a message in the comments! See you next time~ 👋
