https://chatgpt.com/share/6aff2699-a7ac-83e8-845c-6cca3bdbd4dd

trips/city=!{partitionKeyFromQuery:city}/
errors/!{firehose:error-output-type}/

cat > records.json << 'EOF'
[{"Data":"{\"trip_id\":\"T201\",\"city\":\"Delhi\",\"fare_inr\":85}"},
{"Data":"{\"trip_id\":\"T202\",\"city\":\"Mumbai\",\"fare_inr\":340}"},
{"Data":"{\"trip_id\":\"T203\",\"city\":\"Pune\",\"fare_inr\":60}"},
{"Data":"{\"trip_id\":\"T204\",\"city\":\"Delhi\",\"fare_inr\":455}"},
{"Data":"{\"trip_id\":\"T205\",\"city\":\"Mumbai\",\"fare_inr\":58}"},
{"Data":"{\"trip_id\":\"T206\",\"city\":\"Pune\",\"fare_inr\":110}"},
{"Data":"{\"trip_id\":\"T207\",\"city\":\"Delhi\",\"fare_inr\":90}"},
{"Data":"{\"trip_id\":\"T208\",\"city\":\"Mumbai\",\"fare_inr\":385}"}]
EOF
aws firehose put-record-batch --delivery-stream-name trips-to-s3 --records file://records.json --cli-binary-format raw-in-base64-out --query '[FailedPutCount,length(RequestResponses)]' --output text
