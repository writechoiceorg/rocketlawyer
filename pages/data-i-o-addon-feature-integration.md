The Data I/O feature in RocketDocument v2 allows API partners to pre-fill interview answers with existing customer data and retrieve new customer data collected during the interview process. This feature enhances the user experience by reducing redundant data entry and enabling seamless data integration through the Tagged Answer Model (TAM).

## How It Works

Data I/O uses the Tagged Answer Model (TAM) to structure data within interviews. Partners can retrieve the TAM for a specific template, pre-fill fields with customer data, and later retrieve the user's responses after they complete the interview. The key steps in using this feature are retrieving the TAM, submitting the answers, and optionally retrieving or updating the interview data.

## Step 1: Retrieve the TAM for a Template

To start using the Data I/O feature, you first need to retrieve the Tagged Answer Model (TAM) for the interview template you plan to use. The TAM provides the structure you will use to map your customer data. You can do this by sending a request to the [Retrieve Tagged Answer Model for a Template](link). Below, you will find an example request and response for this endpoint:

**Request:**

```curl
curl --request GET \
     --url https://api-sandbox.rocketlawyer.com/rocketdoc/v2/templates/{{templateId}}/tagged-answer-model \
     --header 'accept: application/json' \
     --header 'authorization: Bearer {{generalAccessToken}}'
```

**Response:**
```json
{
  "name": "Employment contract",
  "templateId": "0bf17ad8-a229-1289-ef4a-00b7c9ad9cab",
  "entities": [
    {
      "name": "Formed Entity",
      "class": "Person",
      "roles": [
        "Applicant"
      ],
      "isCyclical": true
    }
  ],
  "fields": [
    {
      "entity": "string",
      "fieldClass": "Entity Type",
      "type": {},
      "value": "string",
      "cycleId": "string"
    }
  ]
}
```

## Step 2: Submit Pre-filled Answers While Creating the Interview

Once you have the TAM, you can pre-fill the interview fields with your customer's data by submitting the answers when creating the interview. You must structure your data following the TAM, but at this point, there is no need to submit an answer to all the fields. During the interview, the answers you have already provided will appear pre-filled for the end user. You can submit your data by sending a request to the [Create an Interview](link) endpoint containing the answers in the `inputData` object, following the structure retrieved in the TAM. The response to this request will be the interview with the informed fields already filled. Below, you will find an example of a request to this endpoint:

**Request:**

```curl
curl --request POST \
     --url https://api-sandbox.rocketlawyer.com/rocketdoc/v2/interviews
     --header 'accept: application/json' \
     --header 'authorization: Bearer {{generalAccessToken}}' \
     --header 'content-type: application/json' \
     --data '
{
  "templateId": "{{templateId}}",
  "answersPayload": {
    "FieldID_1": "Prefilled Answer 1",
    "FieldID_2": "Prefilled Answer 2"
  }
}
'
```

## Optional Steps

### Retrieve Persistent Interview Data

After a persistent interview is completed, you may want to retrieve the user's responses. You can use the [Retrieve Persistent Interview Data](link) for this end. It will return the answers for the interview following the TAM structure.

### Retrieve Ephemeral Interview Data

Similarly to retrieving data from a persistent interview, you can also retrieve the data from an ephemeral interview. To do this, you can use the [Retrieve Ephemeral Interview Data](link) endpoint. It will return the answers following the TAM structure.

### Update an Interview

If you need to update the answers in an ongoing interview, you can do so using the TAM structure. You can use the [Update an Interview](link) endpoint to update all template fields simultaneously.

> **Updating an Interview will Replace Previous Data**
> Updating an interview with this endpoint will replace all existing data for that interview. Make sure this is the intention before proceeding.

## Example TAM files

Below, you can find some example TAM files:

- **Origin file:** This is a sample of a file that is returned when you go through [Step 1: Retrieve the TAM for a Template](#step-1-retrieve-the-tam-for-a-template). [File](link)
- **Input file:** This is a sample of how you should structure the answers when going through [Step 2: Submit Pre-filled Answers While Creating the Interview](#step-2-submit-pre-filled-answers-while-creating-the-interview). [File](link)
- **Output file:** This is a sample of how the answers will be returned if you go through the [Retrieve Persistent Interview Data](#retrieve-persistent-interview-data) or [Retrieve Ephemeral Interview Data](#retrieve-ephemeral-interview-data). [File](link)