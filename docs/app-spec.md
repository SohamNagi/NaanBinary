
# App description

**NaanBinary** is a student life app that turns messy academic information into one clean hub students can use every day. It combines a LaurierFlow-style course and professor ratings explorer with automation that saves time and reduces stress. Students can upload or scan a transcript to generate an organized term-by-term dashboard showing courses taken, grades, credits, and GPA trends. They can upload or scan course outlines to extract deadlines and push them into Google Calendar after a quick review step. FlowSched also helps students stay on track day-to-day with GPS “leave now” reminders and a campus-aware chatbot that answers questions using the student’s actual data, like upcoming deadlines, next class location, or course history. To increase daily stickiness without adding backend complexity, the app includes an attendance streak tracker and a per-course materials hub where students can save key links, PDFs, and quick camera captures of slides or whiteboards, all organized by course.

## MVP implementation notes (for a fast school project)

- **No local database at first**: Use DataStore + JSON file storage for all user data.
- **Calendar starts as a list**: “Upcoming deadlines” list view in MVP; full calendar later.
- **LLM is core**: Use LLM calls for transcript and outline parsing to reduce manual parsing.
- **Mobile features phased**: Camera + GPS in Phase 2 once MVP is stable.

## Updated feature plan

### Tier 0: Core features already scoped

These are the MVP and should be the main demo.

1. **Course and professor ratings explorer**
   Owner: Shahrukh / Muhammad

* Browse courses and profs
* Ratings: overall, difficulty, workload, usefulness
* Search and filters (course code, prof, “low workload”, “best rated”)
  Complexity: Medium
  Backend: Light, start with seeded data, then add submissions

2. **Transcript upload or scan to term dashboard + GPA + courses taken**
   Owner: Soham

* Upload PDF or scan with camera
* LLM extracts terms, courses, grades, credits, term GPA and cumulative GPA
* Term timeline view (expandable term cards)
* GPA calculator and projected GPA when adding future courses
  Complexity: Medium
  Backend: Minimal, store JSON locally, optional sync later

3. **Campus-aware chatbot**
   Owner: Ammar

* Chat UI + suggested prompts
* Grounded answers from app data, read-only
* If unsure, routes user to the right screen instead of guessing
  Complexity: Medium
  Backend: Minimal

4. **GPS “leave now” reminders**
   Owner: Aastha

* Set class locations (pin, building selection)
* Notify based on distance + buffer setting
  Complexity: Medium
  Backend: None

5. **Course outline scan to deadlines (calendar list first, sync later)**
   Owner: Faizaan

* Upload PDF or scan outline (file upload first; in-app scan later)
* LLM extracts assignment names, due dates, optional weight
* Review screen before creating events
* **MVP:** deadlines appear in an “Upcoming” list
* **Later:** Google OAuth + calendar insertion, tagged by course
  Complexity: Medium to High
  Backend: Light

---

### Tier 1: Added stickiness features

Low backend, high value. These should be quick wins.

6. **Attendance streak tracker**

* One tap “Attended” per class session
* Streak per course and weekly summary
* Optional nudge if user misses multiple sessions
  Complexity: Low
  Backend: None, local storage

7. **Course materials hub**

* Per course page to save: links (LMS, Piazza, textbook), files, notes
* Quick access section for most used links
* Search across materials
  Complexity: Low to Medium
  Backend: None, local storage

8. **Camera quick capture integrated into materials hub**

* Snap slides or whiteboards
* Auto-crop and enhance, then save directly under a course
* Tag and search later
  Complexity: Medium
  Backend: None

---

### Tier 2: Optional extras if time remains

Good polish and demo impact without heavy backend.

9. **Unified “Today” view**

* Next class + leave time
* Deadlines in next 48 hours
* Quick actions: open map, open calendar, start focus session
  Complexity: Low

10. **Grade goal planner**

* Set target grade per course
* If outline has weights, compute what is needed on remaining assessments
  Complexity: Medium

11. **Projected GPA simulator**

* Add planned courses with expected grade and credits
* Live projected term and cumulative GPA
  Complexity: Low
