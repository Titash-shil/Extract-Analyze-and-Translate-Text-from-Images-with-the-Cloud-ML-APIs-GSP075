# Extract, Analyze, and Translate Text from Images with the Cloud ML APIs || [GSP075](https://www.cloudskillsboost.google/focuses/1836?parent=catalog) ||

## # Like, comment, share & Don't forget to subscribe [Qwiklab_Explorers](https://youtube.com/@qwiklabexplorers?si=QGN7mY2Sn9iobmuz) 👍😄🤝

---
## ⚠️ **Disclaimer:**
#### This script and guide are provided for educational purposes to help you understand the lab process. Please ensure you understand the steps before using any scripts. Before using the script, I encourage you to open and review it to understand each step.The goal is to help you learn how to complete the labs effectively while following Qwiklabs' terms of service and YouTube's community guidelines.
---

 - ### Copy & Run the Commands in Cloud Shell Terminal :

```
#----------------------------------------------------start--------------------------------------------------#

echo "${YELLOW}${BOLD}Starting${RESET}" "${GREEN}${BOLD}Execution${RESET}"

gcloud alpha services api-keys create --display-name="awesome" 

KEY_NAME=$(gcloud alpha services api-keys list --format="value(name)" --filter "displayName=awesome")

API_KEY=$(gcloud alpha services api-keys get-key-string $KEY_NAME --format="value(keyString)")

export PROJECT_ID=$(gcloud config list --format 'value(core.project)')

export PROJECT_NUMBER=$(gcloud projects describe $DEVSHELL_PROJECT_ID --format="value(projectNumber)")

gcloud storage buckets create gs://$DEVSHELL_PROJECT_ID-bucket --project=$DEVSHELL_PROJECT_ID

gsutil iam ch projectEditor:serviceAccount:$PROJECT_NUMBER@cloudbuild.gserviceaccount.com:objectCreator gs://$DEVSHELL_PROJECT_ID-bucket

curl -LO raw.githubusercontent.com/QUICK-GCP-LAB/2-Minutes-Labs-Solutions/main/Extract%2C%20Analyze%2C%20and%20Translate%20Text%20from%20Images%20with%20the%20Cloud%20ML%20APIs/sign.jpg

gsutil cp sign.jpg gs://$DEVSHELL_PROJECT_ID-bucket/sign.jpg

gsutil acl ch -u AllUsers:R gs://$DEVSHELL_PROJECT_ID-bucket/sign.jpg

touch ocr-request.json

tee ocr-request.json <<EOF
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://$DEVSHELL_PROJECT_ID-bucket/sign.jpg"
          }
        },
        "features": [
          {
            "type": "TEXT_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
EOF

curl -s -X POST -H "Content-Type: application/json" --data-binary @ocr-request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}

curl -s -X POST -H "Content-Type: application/json" --data-binary @ocr-request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY} -o ocr-response.json

touch translation-request.json

tee translation-request.json <<EOF
{
  "q": "My Name is MD_SOHRAB",	
  "target": "en"
}
EOF

STR=$(jq .responses[0].textAnnotations[0].description ocr-response.json) && STR="${STR//\"}" && sed -i "s|your_text_here|$STR|g" translation-request.json

curl -s -X POST -H "Content-Type: application/json" --data-binary @translation-request.json https://translation.googleapis.com/language/translate/v2?key=${API_KEY} -o translation-response.json

cat translation-response.json

touch nl-request.json.json

tee nl-request.json <<EOF
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"your_text_here"
  },
  "encodingType":"UTF8"
}
EOF

STR=$(jq .data.translations[0].translatedText  translation-response.json) && STR="${STR//\"}" && sed -i "s|your_text_here|$STR|g" nl-request.json

curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @nl-request.json

  echo "${RED}${BOLD}Congratulations${RESET}" "${WHITE}${BOLD}for${RESET}" "${GREEN}${BOLD}Completing the Lab !!!${RESET}"

#-----------------------------------------------------end----------------------------------------------------------#
```

---

## Congratulations ..!!🎉  You completed the lab shortly..😃💯

## *Well done..!* 👏

## Thank you for visiting.... :) 🗯️

## [Qwiklab_Explorers](https://youtube.com/@qwiklabexplorers?si=QGN7mY2Sn9iobmuz)

## Join to our community [Digital Dominators](https://linktr.ee/digital_dominators)
