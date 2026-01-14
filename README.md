# Problem Statement 3: AI-Powered Form Filling Assistant for Indian Citizen Services

An **AI-powered form-filling assistant** web interface the **GovAI-Sub (Governmental Form Submissions via AI)** integrating the **IVAN AI (Indic-Vision-ASR-NER AI)** framework for the form-automation was developed under the Intel Unnati Industrial Training Program 2025.

## The Accessibility Gap in Digital Governance
Government paperwork in India often becomes difficult for many citizens because of:

- Long and complex form formats
- Unclear and complex instructions by officials 
- Technical terminology and language barriers  
- Low digital literacy  
- Physical accessibility issues, especially in rural areas
- Physical and age related disabilities

While AI-based digital automation can simplify this process, many existing solutions rely on heavy image/layout-based, agentic, static or black-box architectures that are hard to extend and unsuitable for edge deployment raising privacy concerns. Progress is further limited by Indic and Image data scarcity and an English-centric design, which reduces practicality and inclusiveness for India’s multilingual population.

## Proposed solution

To address this, GovAI-Sub with the IVAN AI framework implements a **multimodal pipeline** that accepts two types of inputs:

- Uploaded document images/PDFs    
- Voice-based inputs in Indic or code-mixed speech  

OCR and ASR components extract raw text, a translation module normalizes multilingual/code-mixed content into a simpler unified text space and a NER module identifies key entities such as names, addresses, IDs and dates (only from OCR texts, ASR does not need an explicit NER model), which are then auto-mapped to corresponding government form fields.

There are three types of synthetically designed forms available in the GovAI-Sub web app replicating real Government Forms.
The template of the forms are attached [here](./Project%20Form%20Templates/).

The IVAN AI system is implemented in Python3 surrounding various NLP technologies like OCR, ASR, Machine Translation and NER. SQLite3 is used as the backend database for user accounts, sessions and form data. The GovAI-Sub web app backend was developed in Python3 while the Interface was developed using HTML, CSS and JavaScript. Overall, the project explores how multilingual, multimodal AI can make Indian e-governance more accessible by automating large parts of the form-filling workflow for everyday users.

## Core AI components

1. **IVAN-OCR**  
   Hybrid OCR combining PaddleOCR text-detection (v3.3.2) and Surya-OCR text-recognition (v0.17.0) for Indic, code-mixed and noisy documents. PaddleOCR was developed on PaddlePaddle-GPU framework (v3.2.1) and Surya-OCR was developed on PyTorch framework (v2.8.0+cu126).

2. **IVAN-ASR**  
   Speech module based on OpenAI Whisper Large-v3 (v20250625) for multilingual and code-mixed Indic speech transcription.

3. **IVAN-Translator**  
   Translation module integrating PolyGlot (v16.7.4) for language detection with `nllb-200-distilled-600M` from transformers (v4.57.1) for lightweight multilingual translation.

4. **IVAN-NER**  
   BERT-Doc-NER-Max, a domain-adapted BERT-Large NER model from transformers (v4.57.1) fine-tuned on a custom CoNLL-style dataset of government-specific entities.

## Pipelines

Two end-to-end pipelines are designed around these models to support real usage scenarios:

1. **OCR → Translation → NER pipeline**  
   For document and handwritten inputs.

2. **ASR → Translation pipeline**  
   For voice-based inputs.

## GovAI-Sub Web Application

The web app backend was developed using Django framework (v5.2.7) in Python3, the backend Database was implemented using SQLite3, the Interface was developed using HTML, CSS and JavaScript while a Virtual Buffer was implemented in the web app using Redis (v7.0.1). The overall implementation helped to develop a smart, user-friendly, multimodal and multilingual digital platform for easily accessible and adaptable form-automation. 

## Web App user flow

The web application follows a simple, user-friendly flow for accessing AI-assisted government forms:

1. **Public Home Page**  
   - Lists all available forms such as Electoral-ID Card (F1), Retirement Subsidy Claim (F2), and International Travel Permit (F3).  
   - Header shows **Log In** and **Register** buttons.  
   - Clicking any form while not logged in redirects the user to the login page.

2. **Registration Page**  
   - New users provide first name, last name, phone number, email and password to create an account on GovAI-Sub.  
   - On successful registration, users are redirected to the login page.  
   - If a user is already registered, a “User already registered!” message is shown with a link to the login page and an option to return to the public home.

3. **Login Page**  
   - Users can log in using their EmailId and password.  
   - Successful login creates a new Session and redirects to the **Home Page** where forms can be accessed and filled.  
   - Unregistered users see a “User not registered!” message with a link to the registration page and a link back to the public home.
  
4. **Private Home Page (Home Page)**
   - Lists all available forms as in Public Home Page.
   - Header shows **Help** option and various accessability options like **Profile**, **Submission History** and **Log Out** redirecting to their respective pages.
   - Clicking on any form opens the **Language & Modality Page** of that form.

5. **Language & Modality Page**  
   - Prompts user to choose form and interface language and input modality (document or voice).  
   - Not all forms are available in all languages, supported languages depend on the specific form, region and government policies.  
   - The system accepts uploads of scanned documents and voice inputs in any supported language and uses AI (OCR, ASR, translation, NER) to extract and map information into the correct form fields.
   - After successful language and modality selection followed by proper document uploads, the **Next** button redirects to the **Review, Edit and Submit Page**.
  
6. **Review, Edit and Submit Page**
   - The auto-filled form is previewed to the user in editable mode to make corrections if needed.
   - Once satisfied, the user can **Submit** the form, that stores the form details in database for future use.
   - After submission the user is auto logged-out to the Public Home Page, terminating the Session.
  
7. **Profile Page**  
   - From the Home Page, users can open the Profile page via the User dropdown to view their stored details such as name, contact information, demographic details and profile picture.  
   - The Profile page is read-only and includes a Home button that navigates back to the User Home page, keeping the user within the logged-in flow.

8. **Submission History Page**  
   - After verifying the auto-filled details on the preview page, the user submits the form, creating a new submission record in the system.  
   - From the User Home, the "User" dropdown provides access to the Submission History page, which lists all past submissions with date and time and offers Download Details for each in PDF form, along with a Home button to return to the User Home page.
  
The detailed description of the IVAN AI framwwork and the GovAI-Sub web app have been provided in the Project Report and Project Presentation (here)[Project Documentation], and the practical preview of both the systems are available (here)[Source Code Recordings].

