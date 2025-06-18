***

### `HelloWord-CHECKLIST-17jun2025-09h30.md`

This checklist is designed to be executed by a team of weak agentic coding LLMs. Each task is granular, specific, and accompanied by a suggested prompt that provides the necessary context for code generation.

```markdown
# HelloWord: Liturgical Companion - Agentic Implementation Checklist

**Document Version:** 1.0
**Date:** 18 June 2025
**Target:** Small, Weak Agentic Coding LLMs

---

## Stream A: Prompt Engineering & Repo Bootstrap (Window: 0-3h)

- [ ] **Task A.1: Initialize Project Structure**
    - **Prompt:** `Initialize a new React Native project named "HelloWord" using the TypeScript template. Then, create the following directory structure inside the 'src' folder: /assets, /components, /core, /hooks, /navigation, /platforms, /screens, /store, /types, /utils. Inside 'src/core', create a 'services' directory.`

- [ ] **Task A.2: Configure Web Support with Vite**
    - **Prompt:** `Install 'vite', '@vitejs/plugin-react', 'vite-plugin-react-native-web', 'react-dom', and 'react-native-web'. Create a 'vite.config.ts' file in the root directory. Configure it to use the React plugin and resolve '.web.tsx', '.web.ts' extensions first. Create a root 'index.html' file that loads a script from '/src/index.web.js'.`

- [ ] **Task A.3: Set up Tailwind CSS**
    - **Prompt:** `Install 'tailwindcss' and 'nativewind'. Initialize a Tailwind configuration file ('tailwind.config.js'). Configure the 'content' array to scan all '.tsx' and '.ts' files within the 'src' directory for class names. Set up the NativeWind Babel plugin.`

- [ ] **Task A.4: Create Core Type Definitions**
    - **Prompt:** `Create the file 'src/core/types/liturgical.ts'. In this file, define the following TypeScript interfaces and enums:
      - enum 'LiturgicalSeason' with values ADVENT, CHRISTMASTIDE, etc.
      - interface 'BilingualText' with fields: latin (string), english (string), isRubric (boolean, optional).
      - interface 'LiturgicalDay' with fields: date (string), season (LiturgicalSeason), celebration (string, optional), rank (number), color (string), commemorations (string[]).
      - interface 'VoiceNote' with fields: id (string), date (string), title (string), filePath (string), duration (number), transcription (string, optional).`

- [ ] **Task A.5: Create Platform Adapter Interfaces**
    - **Prompt:** `Create the file 'src/core/types/services.ts'. In this file, define the following TypeScript interfaces:
      - 'IStorageService' with methods: initialize(), executeQuery(sql, params), transaction(callback).
      - 'IAudioRecorder' with methods: record(), stop(), play(filePath).
      - 'IDeviceInfo' with methods: getPlatform(), isFoldable().`

---

## Stream B: Core Logic Services (Window: 3-12h)

- [ ] **Task B.1: Implement Calendar Service**
    - **Prompt:** `Create the file 'src/core/services/calendarService.ts'. Import the 'LiturgicalDay' and 'LiturgicalSeason' types. Create a class 'LiturgicalCalendar' with a static method 'getDayInfo(date: string): Promise<LiturgicalDay>'. Implement basic logic to determine the season and a placeholder celebration based on the input date string.`

- [ ] **Task B.2: Implement Text Service**
    - **Prompt:** `Create the file 'src/core/services/textService.ts'. It should have a class 'TextService' that takes an 'IStorageService' instance in its constructor. Implement a method 'getMassProper(date: string): Promise<BilingualText[]>'. This method should construct and execute a SQL query to fetch all parts of the mass for a given day's celebration from the 'mass_texts' table.`

- [ ] **Task B.3: Implement Data Manager**
    - **Prompt:** `Create the file 'src/core/services/dataManager.ts'. Create a class 'DataManager' that takes an 'IStorageService' in its constructor. Implement an 'initialize()' method that calls the storage service's 'initialize()' method and runs the CREATE TABLE SQL statements for 'calendar_days', 'mass_texts', 'office_texts', and 'voice_notes'. The SQL should be stored as constants within the file.`

---

## Stream C: Storage & Offline Layer (Window: 6-15h)

- [ ] **Task C.1: Implement Native SQLite Storage Adapter**
    - **Prompt:** `Create the file 'src/platforms/native/sqliteStorage.ts'. Install 'react-native-sqlite-storage'. Implement the 'IStorageService' interface. The 'initialize' method should open the database. The 'executeQuery' method should run a SQL query using the library's transaction API.`

- [ ] **Task C.2: Implement Web IndexedDB Storage Adapter**
    - **Prompt:** `Create the file 'src/platforms/web/indexedDbStorage.ts'. Install 'dexie'. Implement the 'IStorageService' interface using Dexie. The 'initialize' method should define the database schema with object stores for 'calendar_days', 'mass_texts', etc., matching the main schema. The 'executeQuery' method will be a placeholder throwing a 'NotImplemented' error, as Dexie uses a different access pattern.`

- [ ] **Task C.3: Create Storage Service Factory**
    - **Prompt:** `Create 'src/platforms/storageFactory.ts'. This file should have a function 'createStorageService(): IStorageService'. Use 'Platform.OS' to conditionally import and return an instance of 'NativeStorageService' for native platforms and 'WebStorageService' for the web.`

- [ ] **Task C.4: Configure Web Service Worker**
    - **Prompt:** `Install 'workbox-cli'. Create a 'service-worker.js' file in the 'public' directory. Configure Workbox to precache all static assets (JS, CSS) from the build output. Add a runtime caching strategy for font files and images using a 'StaleWhileRevalidate' strategy.`

---

## Stream D: UI Assembly & Navigation (Window: 9-24h)

- [ ] **Task D.1: Set up Navigation**
    - **Prompt:** `Install '@react-navigation/native', '@react-navigation/native-stack', and '@react-navigation/bottom-tabs'. Create 'src/navigation/AppNavigator.tsx'. Implement a BottomTabNavigator with tabs for 'Home', 'Mass', 'Office', and 'Journal'. Each tab should point to a placeholder screen component.`

- [ ] **Task D.2: Create Bilingual Text Component**
    - **Prompt:** `Create 'src/components/BilingualText.tsx'. It should accept a prop 'item: BilingualText'. Use Tailwind CSS classes to render the 'latin' text in a larger font and the 'english' text below it in a smaller, gray font. If 'item.isRubric' is true, render both texts in italic and red.`

- [ ] **Task D.3: Create Home Screen**
    - **Prompt:** `Create 'src/screens/HomeScreen.tsx'. The component should display the current date. It should have buttons styled with Tailwind CSS to navigate to "Mass of the Day" and each of the canonical hours ("Lauds", "Vespers", etc.).`

---

## Stream E: Voice Journal & Reminders (Window: 15-30h)

- [ ] **Task E.1: Implement Redux Journal Slice**
    - **Prompt:** `Create 'src/store/journalSlice.ts' using Redux Toolkit's 'createSlice'. The slice should manage an array of 'VoiceNote' objects. Create async thunks for 'loadJournalEntries', 'addJournalEntry', and 'deleteJournalEntry' that interact with the 'IStorageService'.`

- [ ] **Task E.2: Implement Native Audio Recorder Adapter**
    - **Prompt:** `Create 'src/platforms/native/audioRecorder.ts'. Install 'react-native-audio-recorder-player'. Implement the 'IAudioRecorder' interface using this library.`

- [ ] **Task E.3: Implement Web Audio Recorder Adapter**
    - **Prompt:** `Create 'src/platforms/web/audioRecorder.ts'. Implement the 'IAudioRecorder' interface using the browser's 'MediaRecorder' API. The 'record' method should capture audio data into a Blob, and the 'stop' method should finalize it.`

- [ ] **Task E.4: Implement Journal Recorder UI**
    - **Prompt:** `Create 'src/components/JournalRecorder.tsx'. This component should be a modal containing a title, the selected text context, and buttons for "Record", "Stop", and "Save". When recording, display a timer. On "Save", it should dispatch the 'addJournalEntry' thunk from the journal slice.`

---

## Stream F: Integrated Testing & Hardening (Window: 24-42h)

- [ ] **Task F.1: Write Unit Test for Calendar Service**
    - **Prompt:** `Install 'jest' and 'ts-jest'. Create 'src/core/services/calendarService.test.ts'. Write a unit test for the 'LiturgicalCalendar.getDayInfo' method. Mock the date to '2025-12-25' and assert that the returned 'season' is 'CHRISTMASTIDE' and the 'celebration' is 'Nativity of the Lord'.`

- [ ] **Task F.2: Write Component Test for BilingualText**
    - **Prompt:** `Install '@testing-library/react-native'. Create 'src/components/BilingualText.test.tsx'. Write a test that renders the 'BilingualText' component with sample Latin and English text and asserts that both texts are visible.`

- [ ] **Task F.3: Set up Linting**
    - **Prompt:** `Install 'eslint' and '@typescript-eslint/parser'. Create an '.eslintrc.js' configuration file. Add rules to enforce no unused variables, prefer const over let, and require explicit return types for functions.`

---

## Stream G: Beta Cut & Production Launch (Window: 42-48h)

- [ ] **Task G.1: Configure Android Release Build**
    - **Prompt:** `In 'android/app/build.gradle', configure a signing configuration for release builds. Create a 'keystore.properties' file and add it to '.gitignore'. Document the steps required to generate a signed Android App Bundle (AAB).`

- [ ] **Task G.2: Configure Web PWA Manifest**
    - **Prompt:** `Create a 'public/manifest.json' file. Fill in the 'short_name' as 'HelloWord', the full 'name' as 'HelloWord: Liturgical Companion', and set 'display' to 'standalone'. Add paths to 192x192 and 512x512 app icons.`

- [ ] **Task G.3: Create GitHub Actions Workflow for CI**
    - **Prompt:** `Create a '.github/workflows/ci.yml' file. Configure a workflow that triggers on every push to the 'main' branch. The workflow should have jobs to install dependencies, run the linter ('npm run lint'), and run all tests ('npm test').`

- [ ] **Task G.4: Create GitHub Actions Workflow for Web Deployment**
    - **Prompt:** `Create a '.github/workflows/deploy-web.yml' file. Configure a workflow that triggers on a push to the 'main' branch. It should build the web application ('npm run build:web') and then deploy the contents of the 'dist' folder to GitHub Pages.`
