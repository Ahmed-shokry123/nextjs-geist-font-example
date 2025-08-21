```markdown
# Detailed Implementation Plan for the Content Calendar Application

This plan outlines the creation of a monthly content calendar tool where the user enters their niche and target audience, and the app outputs a generated calendar with daily post ideas, captions, hashtags, suggested visuals, promotional post drafts, posting-time insights, and performance analysis insights. The tool will be integrated into the existing Next.js project using modern, clean UI components with Tailwind CSS and React.

---

## Files to be Created or Modified

- **New UI Page:**  
  - **File:** `src/app/content-calendar/page.tsx`  
    - Displays the form for input and renders the generated calendar.

- **New API Route:**  
  - **File:** `src/app/api/generate-calendar/route.ts`  
    - Processes the POST request from the UI and connects with the LLM API to generate the content calendar.

- **New UI Component:**  
  - **File:** `src/components/ui/ContentCalendarDisplay.tsx`  
    - Renders the calendar output in a modern, card- or table-based layout.

- **Optional Utility Functions:**  
  - **File:** `src/lib/utils.ts`  
    - Add helper functions for parsing and formatting the LLM response if needed.

- **Global Styles (if required):**  
  - **File:** `src/app/globals.css`  
    - Although Tailwind is used, update if any additional custom styles are needed.

---

## Step-by-Step Outline

1. **Create the Content Calendar UI Page (`src/app/content-calendar/page.tsx`):**
   - **Form Creation:**  
     - Implement a modern form with inputs for “niche” and “target audience.”
     - Use Tailwind CSS classes for layout, spacing, and typography.
   - **Input Validation:**  
     - Validate that both fields are not empty before submission.
   - **Submission Handling:**  
     - On form submit, trigger a fetch (POST) request to `/api/generate-calendar` with the input data.
     - Display a loading indicator while waiting for the response.
   - **Error Handling:**  
     - Show a clear error message if the API call fails.
   - **Result Rendering:**  
     - On successful response, pass the calendar data to the new ContentCalendarDisplay component for presentation.

2. **Develop the API Route (`src/app/api/generate-calendar/route.ts`):**
   - **HTTP Method Check:**  
     - Ensure the endpoint only accepts POST requests; return a 405 error for others.
   - **Request Parsing and Validation:**  
     - Parse the JSON body and confirm that `niche` and `targetAudience` keys are provided; respond with a 400 error if validation fails.
   - **LLM Integration:**  
     - Construct a clear prompt instructing the LLM (using the OpenRouter API endpoint) to generate a detailed monthly content calendar.  
       _Example prompt:_  
       “Generate a monthly content calendar for a [niche] audience targeting [targetAudience]. The calendar should include daily post ideas, promotional post drafts for upcoming events, trending hashtags, best posting times insights, as well as strategies for repurposing content and performance analysis (engagement rates, follower growth, audience feedback).”
     - Use a POST request with appropriate headers (including an authorization header if an API key is provided via environment variables).  
     - Use the model `anthropic/claude-sonnet-4` (the default model) in the request payload.
   - **Error Handling:**  
     - Use try-catch blocks to capture any issues during the API call, and return a proper JSON error response with status code.
   - **Response Processing:**  
     - Format the LLM response into structured JSON (for example, an array of day objects each containing date, post idea, caption, hashtags, and suggested visual info).
     - Send the structured JSON back to the client.

3. **Build the Content Calendar Display Component (`src/components/ui/ContentCalendarDisplay.tsx`):**
   - **Component Structure:**  
     - Accept a prop for the calendar data.
     - Render a modern card or grid layout for each day’s content using clean typography and balanced spacing.
   - **Visuals for Suggested Post Themes:**  
     - For posts that suggest a visual, include an `<img>` tag with a placeholder URL such as:  
       ```
       <img 
         src="https://placehold.co/800x600?text=Suggested+visual+for+post+theme" 
         alt="Detailed+description+of+the+suggested+visual+representing+the+post+theme" 
         onerror="this.onerror=null;this.src='https://placehold.co/800x600?text=Image+not+available';" 
       />
       ```
   - **Additional Sections:**  
     - Optionally, create separate sections or tabs for promotional posts, trending hashtags, and analytics insights if the generated data is rich enough.
   - **Error/Empty States:**  
     - Display an appropriate message or placeholder if no calendar data is available.

4. **Update Utility Functions (in `src/lib/utils.ts`) [Optional]:**
   - Add helper methods to parse and transform the raw text response from the LLM into a structured JSON object.
   - Ensure error handling for unexpected data formats.

5. **Testing and Quality Assurance:**
   - **API Testing (using curl):**  
     - Example command:
       ```bash
       curl -X POST http://localhost:3000/api/generate-calendar \
         -H "Content-Type: application/json" \
         -d '{"niche": "Fitness", "targetAudience": "Young Adults"}'
       ```
       - Validate that the API returns a well-structured JSON object.
   - **UI Testing:**  
     - Run the Next.js development server and verify that the new page renders correctly.
     - Test form submissions and verify that loading, error, and success states work as expected.
   - **Error Handling:**  
     - Verify that all error messages (both client-side and server-side) are clear and actionable.

---

## Summary

- A new page (`src/app/content-calendar/page.tsx`) provides a modern UI for entering niche and target audience details.
- The API route (`src/app/api/generate-calendar/route.ts`) validates input and integrates with an LLM via the OpenRouter endpoint using the `anthropic/claude-sonnet-4` model to generate calendar data.
- A new UI component (`src/components/ui/ContentCalendarDisplay.tsx`) displays the generated content in a clean, card/grid layout with detailed placeholders for suggested visuals.
- Optional utility functions in `src/lib/utils.ts` will process and format the LLM response.
- The design employs modern Tailwind CSS styling, robust error handling, and clear user feedback throughout the application.
- The plan ensures realistic features including daily post ideas, promotional content drafts, trending hashtag lists, posting time insights, and performance analysis suggestions.
