# Product Requirements Document (PRD)

## Babcock Campus Ride: A Ride-Hailing App for Babcock University Tricycles
*(working title)*

**Group Name:** All Progressive Congress (APC)

**Group Members:**

| Name | Matric No. | Role |
|---|---|---|
| Bamigboye Oluwasomidipupo Emmanuel | 25/1992 | Team Lead |
| Dare Amos | 25/1562 | Member |
| Gospel Princewill | 25/1564 | Member |
| Ikediasor David | 25/0067 | Member |
| Beyioku-Alase | 25/1341 | Member |
| Friday Chidoye | 25/1195 | Member |

**Version:** 1.0 (Draft)
**Date:** November 2026

---

## 1. Overview

Commercial tricycles are the main way students move around Babcock University, but they are hard to find at the moments they are needed most (class breaks, early mornings, late evenings). This project is a campus-only, Uber-style app that lets a student request a tricycle and lets nearby riders accept the request.

## 2. Problem Statement

- Students waste time waiting or searching for a tricycle, and are often late to lectures.
- Riders have no reliable way to know where demand is, so supply and demand are mismatched.
- There is no accountability or record of trips (which rider, which plate number, how much).

## 3. Goals and Non-Goals

### Goals
1. Reduce the time a student spends finding a tricycle.
2. Give riders a steady stream of ride requests.
3. Provide a simple, trustworthy way to identify the rider and track trip status.
4. Deliver a working MVP within one semester.

### Non-Goals (for v1)
- In-app payments (cash on delivery of the ride for now)
- Ratings and reviews
- Live GPS map tracking and turn-by-turn routing
- Surge or dynamic pricing
- Coverage outside Babcock University campus

## 4. Target Users

| User | Description | Key Needs |
|---|---|---|
| **Student (Passenger)** | Babcock student or staff moving around campus | Fast, reliable ride; know who is picking them up; fixed, fair fare |
| **Rider (Driver)** | Commercial tricycle operator on campus | Receive requests, accept easily, minimal data/phone usage |
| **Admin** (optional, later) | Group or school-approved coordinator | Approve riders, view trip records |

## 5. User Stories

**Student**
- As a student, I can sign up using my Babcock email or matric number so only campus members use the app.
- As a student, I can choose a pickup and drop-off point from a list of campus locations.
- As a student, I can see the fare before requesting.
- As a student, I can see my rider's name, plate number, and trip status.
- As a student, I can cancel a request before a rider arrives.

**Rider**
- As a rider, I can go online or offline.
- As a rider, I can see incoming ride requests and accept or decline.
- As a rider, I can see the pickup and drop-off locations and update the trip status.
- As a rider, I can mark a trip as completed.

## 6. Functional Requirements (MVP)

| ID | Requirement | Priority |
|---|---|---|
| FR-1 | User registration and login with role selection (student / rider) | Must |
| FR-2 | Student identity verification via Babcock email or matric number | Must |
| FR-3 | Ride request using fixed pickup and drop-off dropdowns of campus locations (hostels, lecture halls, cafeteria, gates, etc.) | Must |
| FR-4 | Fixed fare table between known locations, shown before request | Must |
| FR-5 | Request broadcast to online riders; first to accept gets the ride | Must |
| FR-6 | Realtime trip status: Requested → Accepted → Arriving → On Trip → Completed (or Cancelled) | Must |
| FR-7 | Student sees rider name, phone, and plate number after acceptance | Must |
| FR-8 | Rider online/offline toggle | Must |
| FR-9 | Rider accept / decline / complete trip controls | Must |
| FR-10 | Trip history for students and riders | Should |
| FR-11 | Cancel ride (with simple rules) | Should |
| FR-12 | Scheduled rides (for peak times such as early morning) | Could |
| FR-13 | Rider demand view by location | Could |
| FR-14 | Ratings, in-app payments, live map tracking | Won't (v1) |

**Design decision:** Fixed location dropdowns replace free-form maps. This removes geocoding and routing complexity and makes pricing straightforward on a small campus.

## 7. Non-Functional Requirements

- **Performance:** Ride requests should reach riders within a few seconds.
- **Low data usage:** The rider interface must work on cheap phones and limited data.
- **Reliability:** The app should handle poor network gracefully (retry, clear error messages).
- **Security:** Authenticated users only; passwords hashed; role-based access (students cannot use rider functions and vice versa).
- **Privacy:** Only share a rider's or student's contact details once a ride is accepted.
- **Usability:** Requesting a ride should take no more than 3 taps after login.

## 8. Proposed Tech Stack

| Layer | Option |
|---|---|
| Frontend | React Native (Expo) or Flutter; a PWA is an acceptable fast start |
| Backend | Node.js + Express, or Firebase for auth, database, and realtime |
| Database | Firestore or PostgreSQL |
| Realtime | Firestore listeners or WebSockets (Socket.io) |
| Hosting | Firebase / Render / similar free tier |

*The final stack will be agreed on based on the group's comfort and skills.*

## 9. Data Model (Draft)

**users**: `id`, `name`, `email`, `phone`, `role` (student | rider), `matricNo`, `createdAt`

**riders**: `userId`, `plateNumber`, `isOnline`, `isApproved`

**locations**: `id`, `name`, `category`

**fares**: `pickupId`, `dropoffId`, `amount`

**rides**: `id`, `studentId`, `riderId`, `pickupId`, `dropoffId`, `status`, `fare`, `createdAt`, `acceptedAt`, `completedAt`

## 10. Core User Flow

1. Student logs in and taps **Request Ride**.
2. Student selects pickup and drop-off; the fare is displayed.
3. Student confirms; the request is broadcast to online riders.
4. A rider accepts; the student sees rider name, plate number, and status.
5. Rider marks **Arriving**, then **On Trip**, then **Completed**.
6. Trip is saved to history; student pays cash at the agreed fare.

## 11. Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Riders do not adopt the app (limited smartphones or data) | High | Engage riders early; keep the rider UI very simple; consider an SMS fallback |
| Not enough riders online at peak times | High | Pilot with a small group of willing riders; consider scheduled rides |
| Students bypass the app and flag tricycles directly | Medium | Make the app faster and more reliable than street hailing |
| Scope creep beyond one semester | Medium | Strictly follow MVP list; defer non-goals |
| Poor network on campus | Medium | Lightweight payloads, retry logic |
| Safety and trust concerns | Medium | Show rider identity and plate number; approve riders before they go online |

## 12. Success Metrics

- Average time from request to rider acceptance (target: under 3 minutes)
- Number of registered students and active riders during the pilot
- Percentage of requests that are accepted and completed
- User satisfaction from short surveys of students and riders

## 13. Suggested Team Responsibilities

| Member | Suggested Focus |
|---|---|
| To be assigned | Student app |
| To be assigned | Rider app |
| To be assigned | Backend and database |
| To be assigned | Backend and database (second developer) |
| To be assigned | UI design |
| To be assigned | User research (surveys of students and riders) and testing |
| Bamigboye Oluwasomidipupo Emmanuel (Team Lead) | Coordination, milestones, and integration across all areas |

*Roles to be confirmed by the group.*

## 14. Proposed Milestones

| Phase | Deliverable |
|---|---|
| 1. Research | Survey students and riders; confirm campus locations and fares |
| 2. Design | Wireframes, data model, finalized tech stack |
| 3. Build | Auth, ride request flow, rider accept flow, realtime status |
| 4. Test | Internal testing, then a small pilot with a few riders and students |
| 5. Iterate and Present | Fix issues, prepare demo and documentation |

## 15. Future Enhancements

- In-app payments
- Ratings and reviews
- Live GPS map tracking
- Demand heatmap for riders
- Scheduled and recurring rides
- Admin dashboard and analytics
- Support for other campus transport options
