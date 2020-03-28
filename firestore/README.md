# TempTeam Firestore Schema

## JSON representation

```json
{
	"users": {
		"<uid>": {
			"healthScore": 30,
			"healthResponsibilityScore": 99,
			"socialDistancingScore": 56,
			"history": {
				"birthYear": 1578,
				"hasAsthma": true,
				"hasCOPD": true,
				"hasDiabetes": true,
				"hasHypertension": true,
				"hasHeartDisease": true,
				"hasSeasonalAllergies": true,
				"isImmunocompromised": true,
				"isSmoker": true
			},
			// sub-collection
			"healthRecords": [
				{
					"chills": true,
					"cough": true,
					"diarrhea": true,
					"febrile": true,
					"hasCovid": true,
					"hasFlu": true,
					"headaches": true,
					"malaise": true,
					"myalgias": true,
					"nausea": true,
					"runnyOrStuffyNose": true,
					"shortnessOfBreath": true,
					"sneezing": true,
					"soreThroat": true,
					"subjectiveFever": true,
					"temp": 99,
					"timestamp": "2-7-2020 02:35PM"
				}
			],
			// sub-collection
			"encounters": [
				{
					"entry": "2-5-2020 02:35PM",
					"exit": "2-5-2020 02:36PM",
					"lat": 0,
					"long": 0,
					"uid": "<user-2>"
				}
			]
	}
}
```

## Entities

### User

**Collection:** `/users/{userId}`<br />
**ID:** The user's Firebase auth id.

| Property                    | Type           | Description                                                                   |
| --------------------------- | -------------- | ----------------------------------------------------------------------------- |
| `healthScore`               | number         | Aggregate health score                                                        |
| `healthResponsibilityScore` | number         | How responsible has the individual been with tracking their health?           |
| `socialDistancingScore`     | number         | How responsible has the individual been with practicing social distancing? () |
| `history`                   | Map            | Personal medical history.                                                     |
| `healthRecords`             | sub-collection | Daily healthcare metrics provided by the user.                                |
| `encounters`                | sub-collection | Tracked encounters between the user and another user.                         |

### History

This is a map on the User object. It is a record of the user's health history which is intended to be used in determining how susceptible they are to infection.

| Property               | Type    | Description                                 |
| ---------------------- | ------- | ------------------------------------------- |
| `birthYear`            | number  | Year of birth. _Should we track month/day?_ |
| `hasAsthma`            | boolean |                                             |
| `hasCOPD`              | boolean |                                             |
| `hasDiabetes`          | boolean |                                             |
| `hasHypertension`      | boolean |                                             |
| `hasHeartDisease`      | boolean |                                             |
| `hasSeasonalAllergies` | boolean |                                             |
| `isImmunocompromised`  | boolean |                                             |
| `isSmoker`             | boolean |                                             |

### Health Records

**Collection:** `/users/{userId}/healthRecords/{id}`<br />
**ID:** Random health record id. _Could possibly use the day._
Daily health record input by the user.

| Property            | Type    | Description      |
| ------------------- | ------- | ---------------- |
| `chills`            | boolean |                  |
| `cough`             | boolean |                  |
| `diarrhea`          | boolean |                  |
| `febrile`           | boolean |                  |
| `hasCovid`          | boolean |                  |
| `hasFlu`            | boolean |                  |
| `headaches`         | boolean |                  |
| `malaise`           | boolean | Tired or weak.   |
| `myalgias`          | boolean | Aches and pains. |
| `nausea`            | boolean |                  |
| `runnyOrStuffyNose` | boolean |                  |
| `shortnessOfBreath` | boolean |                  |
| `sneezing`          | boolean |                  |
| `soreThroat`        | boolean |                  |
| `subjectiveFever`   | boolean |                  |
| `temp`              | boolean |                  |
| `timestamp`         | boolean |                  |

### Encounter

**Collection:** `/users/{userId}/encounters/{id}`<br />
**ID:** Random encounter id.
Tracks when another user enters and exits a certain proximity of the user.

| Property | Type      | Description           |
| -------- | --------- | --------------------- |
| `entry`  | timestamp |                       |
| `exit`   | timestamp |                       |
| `lat`    | boolean   | Latitude upon entry.  |
| `long`   | boolean   | Longitude upon entry. |
| `uid`    | boolean   | Id of the other user. |
