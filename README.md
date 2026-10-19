# AI-Driven Public Health Agent

An academic web application prototype that collects a user's self-reported symptoms and optional health information, sends the submission to Google's Gemini API, and presents a generated informational report. The interface supports English, Hindi, and Marathi.

> **Medical safety notice:** This project is an educational prototype, not a medical device or clinical decision-support system. It does not diagnose, triage, or replace a qualified healthcare professional. AI-generated content—including risk levels, possible causes, medication suggestions, and diet plans—may be inaccurate or unsafe. Do not use it to make treatment decisions or delay urgent care. For emergencies, contact local emergency services.

## Features

- Responsive intake form for basic details, symptoms, medical history, and optional vital signs.
- Language preference selection: English, Hindi, or Marathi.
- Server-side call to the Gemini `gemini-2.5-flash` model using the Google Generative AI JavaScript SDK.
- AI report rendered as an EJS page with a summary, possible causes, risk level, precautions, over-the-counter suggestions, a seven-day meal plan, and next steps.
- Express application configured for local Node.js execution and with a Vercel deployment configuration included.

## How it works

1. `GET /` renders the intake form.
2. The browser submits form-encoded data to `POST /analyze`.
3. Express checks that a name and symptoms were supplied, then sends the submitted details and a structured-output prompt to Gemini.
4. The server parses the model's JSON response and renders it with the submitted information in the report page.

The application does not currently persist submissions to a database. Submitted health information is sent to the configured Gemini service to generate a report; review the provider's terms and privacy policies before use.

## Technology

- Node.js (supported range in `package.json`: `>=20.0.0 <25.0.0`)
- Express 5 and EJS
- Google Generative AI JavaScript SDK
- dotenv for local environment configuration
- Tailwind CSS via CDN, with custom page styles

## Repository layout

```text
.
├── app.js                 # Express server and Gemini integration
├── package.json           # Dependencies and Node.js version range
├── package-lock.json      # Locked npm dependency tree
├── vercel.json            # Vercel Node function routing configuration
└── views/
    ├── home.ejs           # Health information intake form
    └── report.ejs         # Generated report page
```

## Prerequisites

- Node.js 20–24 and npm
- A Google Gemini API key with access to the configured model
- Internet access for Gemini and the frontend CDN assets

## Run locally

Run these commands from the project root:

```bash
npm install
```

Create a `.env` file in the project root (or set these environment variables in your shell):

```env
GEMINI_API_KEY=replace_with_your_gemini_api_key
PORT=5000
```

Keep the real key private. Do not put it in browser code, commit it, or paste it into issue reports. `.env` is excluded by `.gitignore`.

Start the server:

```bash
node app.js
```

Open [http://localhost:5000](http://localhost:5000). If `PORT` is set to a different value, use that port instead.

> `package.json` currently has no `start` script. `npm start` will not work unless a start script is added.

## Routes

| Method | Path | Behavior |
| --- | --- | --- |
| `GET` | `/` | Renders the intake page. |
| `POST` | `/analyze` | Validates the required name and symptoms, requests a Gemini report, and renders the report. Missing required fields return HTTP 400; a Gemini request failure returns HTTP 500. |

The form includes name, age, gender, contact details, symptoms, medical history, temperature, pulse, blood pressure, weight, and preferred language. Only the name and symptoms are explicitly validated by the server today; browser-side `required` attributes are not a replacement for server-side validation.

## Configuration and deployment

- `GEMINI_API_KEY` (required): Gemini API key read by the server.
- `PORT` (optional): HTTP port; defaults to `5000` locally.

A `vercel.json` configuration is included, but deployment has not been verified as part of this repository. Configure `GEMINI_API_KEY` in the deployment platform's server-side environment settings, never in client-side assets. Test the deployment, serverless request behavior, view resolution, and provider access before sharing it publicly.

## Known limitations

- No automated tests are configured. The current `npm test` script is a placeholder that exits with an error.
- Input checks are minimal; fields such as age, language, lengths, and formats are not validated on the server.
- Model output is parsed as JSON but is not validated against a schema. Malformed output falls back to a minimal report object, which can leave report sections empty.
- The application has only basic request-failure handling and no dedicated emergency-symptom guidance or clinical review workflow.
- PDF export is not implemented: the report page loads PDF-related browser libraries but does not provide a working download control.
- There is no documented retention, consent, access-control, or comprehensive privacy workflow. Do not submit real or identifiable patient information.
- Vercel deployment and production readiness have not been confirmed.

## Privacy and security

Health details are sensitive. Use fictional sample data for demos and coursework. The app sends entered health details to Google Gemini for generation; avoid entering personally identifiable or real patient information. Follow applicable laws, consent requirements, and provider policies before any real-world use. Keep API credentials in environment variables and rotate a key immediately if it is exposed.

## Project status and contributions

This repository is suitable for learning and demonstration. Contributions that improve server-side validation, output-schema checks, safe failure states, emergency messaging, privacy documentation, tests, and verified deployment are welcome. Any clinical or real-world use requires appropriate expert review, safety controls, and legal/privacy assessment.

## License

`package.json` declares the ISC license. No separate `LICENSE` file is currently present, so check with the project maintainers and add the corresponding license text before redistributing the project.