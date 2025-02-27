# Nudge API Documentation

## Object Data Model

```javascript
{
  type: "nudge",
  eventId: ObjectId,  // Reference to the associated event
  title: String,      // Title of the nudge
  image: String,      // Path to cover image
  scheduledTime: Date, // Time when nudge should be sent
  description: String, // Detailed description
  icon: String,       // Path to icon image
  invitationText: String, // One line invitation text
  createdBy: Number,  // User ID who created the nudge
  status: String,     // Status of the nudge (draft, scheduled, sent)
  createdAt: Date,    // Creation timestamp
  updatedAt: Date     // Last update timestamp
}
```

## API Endpoints

| Request Type | Base URL | API Endpoint | Payload | Description |
|--------------|----------|--------------|---------|-------------|
| GET | /api/v3/app | /nudges?id=:nudge_id | - | Gets a nudge by its unique id |
| GET | /api/v3/app | /nudges?eventId=:event_id | - | Gets all nudges associated with an event |
| GET | /api/v3/app | /nudges?status=scheduled&page=1&limit=10 | - | Gets nudges filtered by status with pagination |
| POST | /api/v3/app | /nudges | title, files[image], files[icon], eventId, scheduledTime, description, invitationText | Creates a new nudge and returns its ID |
| PUT | /api/v3/app | /nudges/:id | Same as POST payload | Updates an existing nudge |
| DELETE | /api/v3/app | /nudges/:id | - | Deletes a nudge based on its Unique Id |

## API Details

### GET /nudges?id=:nudge_id
- Returns a single nudge by its MongoDB ObjectId
- Response includes all nudge details including associated event information

### GET /nudges?eventId=:event_id
- Returns all nudges associated with a specific event
- Results can be paginated using page and limit query parameters
- Sorted by scheduledTime in ascending order

### GET /nudges?status=scheduled&page=1&limit=10
- Returns nudges filtered by their status (draft, scheduled, sent)
- Supports pagination with page and limit parameters
- Default limit is 10 nudges per page
- Sorted by scheduledTime in ascending order

### POST /nudges
- Creates a new nudge
- Requires multipart form data for image and icon uploads
- Required fields:
  - title: String
  - eventId: ObjectId of associated event
  - scheduledTime: ISO DateTime string
  - description: String
  - invitationText: String
- Optional fields:
  - files[image]: Cover image file
  - files[icon]: Icon image file

### PUT /nudges/:id
- Updates an existing nudge
- Accepts same fields as POST
- Only provided fields will be updated
- Returns updated nudge object

### DELETE /nudges/:id
- Permanently deletes a nudge
- Also removes associated files (images)
- Returns success status