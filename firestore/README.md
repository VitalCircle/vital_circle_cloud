# TempTeam Firestore Schema

## JSON representation

```json
{
  "users": {
    "<uid>": {
      "agreements": {
        "acceptedLocationSharing": true,
        "locationSharingDate": "2-7-2020 02:35PM",
        "privacyPolicyDate": "2-7-2020 02:35PM",
        "termsOfServiceDate": "2-7-2020 02:35PM",
      },
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
      "checkins": {
        "YYYY-MM-DD": {
          "feeling": "",
          "temp": 0,
          "subjectiveTemp": "",
          "symptoms": {
            "bodyAches": 0,
            "cough": 0,
            "diarrhea": 0,
            "febrile": 0,
            "feelingIll": 0,
            "headache": 0,
            "nauseaVomiting": 0,
            "oddTaste": 0,
            "oddSmell": 0,
            "runnyNose": 0,
            "shortnessOfBreath": 0,
            "sneezing": 0,
            "soreThroat": 0
          },
          "timestamp": "" // timestamp
        }
      },
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
| `agreements`                | Map            | Record of their acceptance of agreements.                                     |
| `healthScore`               | number         | Aggregate health score                                                        |
| `healthResponsibilityScore` | number         | How responsible has the individual been with tracking their health?           |
| `socialDistancingScore`     | number         | How responsible has the individual been with practicing social distancing? () |
| `history`                   | Map            | Personal medical history.                                                     |
| `checkins`                  | sub-collection | Daily healthcare metrics provided by the user.                                |
| `encounters`                | sub-collection | Tracked encounters between the user and another user.                         |

### Agreements

This is a map on the User object. It is a record of the user's acceptance of agreements.

| Property                  | Type      | Description                                            |
| ------------------------- | --------- | ------------------------------------------------------ |
| `acceptedLocationSharing` | bool      | Have they responded to the location sharing agreement? |
| `locationSharingDate`     | Timestamp | When they accepted the location sharing agreement.     |
| `privacyPolicyDate`       | Timestamp | When they accepted the privacy policy agreement.       |
| `termsOfServiceDate`      | Timestamp | When the accepted the terms of service agreement.      |

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

### Checkin

**Collection:** `/users/{userId}/checkins/{id}`<br />
**ID:** Date of the checkin in the format of `YYYY-MM-DD`. There is only one per day and this lets us directly get the checkin for the given day.
Daily health record input by the user.

| Property         | Type      | Description            |
| ---------------- | --------- | ---------------------- |
| `feeling`        | string    | worse, similar, better |
| `temp`           | double    | number                 |
| `subjectiveTemp` | string    | hot, fine, unsure      |
| `symptoms`       | Map       |                        |
| `timestamp`      | Timestamp |                        |

### Symptoms

This is a map on the Checkin object. The number indicates the severity (0 - none, 1 - mile, 2 - moderate, 3 - severe).

| Property            | Type   | Description |
| ------------------- | ------ | ----------- |
| `bodyAches`         | number |             |
| `cough`             | number |             |
| `diarrhea`          | number |             |
| `febrile`           | number |             |
| `feelingIll`        | number |             |
| `headache`          | number |             |
| `nauseaVomiting`    | number |             |
| `oddTaste`          | number |             |
| `oddSmell`          | number |             |
| `runnyNose`         | number |             |
| `shortnessOfBreath` | number |             |
| `sneezing`          | number |             |
| `soreThroat`        | number |             |

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
