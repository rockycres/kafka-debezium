alter table public student REPLICA IDENTITY FULL;

Invoke-WebRequest "127.0.0.1:8083/connectors" `
      -Method Post -ContentType "application/json" `
      -InFile .\debezium.json -UseBasicParsing

docker run --tty  --network debezium-kafka_default  confluentinc/cp-kafkacat  kafkacat -b kafka:9092 -C  -s key=s -s value=avro  -r http://schema-registry:8081  -t postgres.public.student
