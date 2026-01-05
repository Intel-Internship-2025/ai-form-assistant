# Problem Statement 3: AI-Powered Form Filling Assistant for Indian Citizen Services

An **AI-powered form-filling assistant** web app **IVAN AI (Indic-Vision-ASR-NER AI)** was developed under the Intel Unnati Industrial Training Program 2025.

## The Accessibility Gap in Digital Governance
Government paperwork in India often becomes difficult for many citizens because of:

- Long and complex form formats  
- Technical terminology and language barriers  
- Low digital literacy  
- Physical accessibility issues, especially in rural areas  

While AI-based digital automation can simplify this process, many existing solutions rely on heavy image/layout-based, agentic or black-box architectures that are hard to extend and unsuitable for edge deployment. Progress is further limited by data scarcity and an English-centric design, which reduces practicality and inclusiveness for India’s multilingual population.

## Proposed solution

To address this, IVAN AI implements a **multimodal pipeline** that accepts three types of inputs:

- Uploaded document images/PDFs  
- Uploaded handwritten proformas  
- Voice-based inputs in Indic or code-mixed speech  

OCR and ASR components extract raw text, a translation module normalizes multilingual/code-mixed content into a simpler text space, and a NER module identifies key entities such as names, addresses, IDs and dates, which are then auto-mapped to corresponding government form fields.

The template of the proformas are attached here: [Project Form Templates]

The system is implemented in Python using OCR, ASR, machine translation and NER models. SQLite is used as the backend database for user accounts, sessions and form data. Overall, the project explores how multilingual, multimodal AI can make Indian e-governance more accessible by automating large parts of the form-filling workflow for everyday users.

## Core AI components

1. **IVAN-OCR**  
   Hybrid OCR combining PaddleOCR (detection) and Surya-OCR (recognition) for Indic, code-mixed and noisy documents.

2. **IVAN-ASR**  
   Speech module based on OpenAI Whisper Large-v3 for multilingual and code-mixed Indic speech transcription.

3. **IVAN-Translator**  
   Translation module integrating PolyGlot for language detection with `nllb-200-distilled-600M` for lightweight multilingual translation.

4. **IVAN-NER**  
   BERT-Doc-NER-Max, a domain-adapted BERT-Large NER model fine-tuned on a custom CoNLL-style dataset of government-specific entities.

## Pipelines

Two end-to-end pipelines are designed around these models to support real usage scenarios:

1. **OCR → Translation → NER pipeline**  
   For document and handwritten inputs.

2. **ASR → Translation pipeline**  
   For voice-based inputs.

## Web app user flow

The web application follows a simple, secure flow for accessing AI-assisted government forms:

1. **Public home page (index)**  
   - Lists all available forms such as Electoral-ID Card (F1), Retirement Subsidy Claim (F2), and International Travel Permit (F3).  
   - Header shows **Log In** and **Register** buttons.  
   - Clicking any form while not logged in redirects the user to the login page.

2. **Registration**  
   - New users provide first name, last name, phone number, email, and password to create an account on GovAI-Sub.  
   - On successful registration, users are redirected to the login page.  
   - If a user is already registered, a “User already registered!” message is shown with a link to the login page and an option to return to the public home.

3. **Login**  
   - Users can log in using their EmailId or UserId and password.  
   - Successful login redirects to the **User Home** page where forms can be accessed and filled.  
   - Unregistered users see a “User not registered!” message with a link to the registration page and a link back to the public home.

4. **Language support and form access**  
   - The main interface is in English, but forms are available in multiple Indian languages (currently Hindi, Bengali, Tamil, Telugu, Marathi) in addition to English.  
   - Not all forms are available in all languages; supported languages depend on the specific form and region.  
   - The system accepts uploads of scanned documents, handwritten notes, and voice inputs in any supported language and uses AI (OCR, ASR, translation, NER) to extract and map information into the correct form fields.
  
5. **Profile viewing**  
   - From the User Home, users can open the Profile page via the User dropdown to view their stored details such as name, contact information, demographic details, and profile picture.  
   - The Profile page is read-only and includes a Home button that navigates back to the User Home page, keeping the user within the logged-in flow.

6. **Form submission and history**  
   - After verifying the auto-filled details on the preview page, the user submits the form, creating a new submission record in the system.  
   - From the User Home, the "User" dropdown provides access to the Submission History page, which lists all past submissions with date and time and offers Download Details for each, along with a Home button to return to the User Home page.

