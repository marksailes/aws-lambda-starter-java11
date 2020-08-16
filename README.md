# aws-lambda-starter-java11

Cold Start Times:
net.sailes lambda-structured-logging 867ms 3gb
net.sailes lambda-structured-logging 2187ms 1gb

io.symphonia lambda-logging - 1048ms 3gb
io.symphonia lambda-logging - 2394ms 1gb

Architecture Decision Record (ADR):
- Leverage AWS Lambda startup CPU boost: initialize everything in static blocks & constructor

- Leverage AWS Lambda startup unbilled time: 
    - Do mock DynamoDB GET to initialize marshallers in constructor
    
- Minimize dependencies used & prefer lightweight alternatives
    - Use url-connection-client and exclude apache-client & netty-nio-client
    - No dependency injection unless too many classes
    
- Symphonia logging (logback) over Log4j2: faster startup (~300ms) & smaller (-1 MB)

- Leverage Enhanced DynamoDB client to minimize boilerplate and almost no cold start penalty
    - https://aws.amazon.com/blogs/developer/introducing-enhanced-dynamodb-client-in-the-aws-sdk-for-java-v2/

- Configure as much as possible on DynamoDB (region, credentials lookup, http client) to minimize lookup delay
    - https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/client-configuration-starttime.html
    
- Use ZIP deployment over uber/shaded-jar