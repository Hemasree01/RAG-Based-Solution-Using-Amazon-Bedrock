<h3>RAG-Based Solution Using Amazon Bedrock</h3>

<p>This guide provides step-by-step instructions to build a Retrieval-Augmented Generation (RAG) system using Amazon Bedrock, OpenSearch, and AWS Lambda.</p>

<h2>Steps</h2>

1. Create an S3 Bucket and Upload Documents</br>

<p>Navigate to Amazon S3 in the AWS Console.</br>

Click Create bucket.</br>

Provide a unique bucket name (e.g., rag-knowledge-base).</br>

Choose Region: us-east-1 (Required for Amazon Bedrock).</br>

Disable Block all public access (as needed).</br>

Click Create bucket.</br>

Inside the S3 bucket, click Upload.</br>

Select your PDF, DOCX, TXT, or JSON files and click Upload.</br>

Note the S3 URI (e.g., s3://rag-knowledge-base/documents/).</p></br>

<p>2. Set Up Knowledge Base in Amazon Bedrock</br>

Navigate to Amazon Bedrock.</br>

Ensure you are in the us-east-1 region.</br>

Click on Knowledge Base > Create a knowledge base.</br>

Enter a Name (e.g., rag-kb).</br>

In Data Source, select Amazon S3.</br>

Provide the S3 bucket URI where documents are stored.</br>

Choose Chunking Strategy (Default recommended).</br>

Choose Amazon Titan v2 Embedding Model for embeddings.</br>

Select OpenSearch as the vector store.</br>

Click Create.</br>

Once the knowledge base is created, click Sync Now.</br>

Wait for the synchronization to complete.</br>

The documents are now indexed and vectorized in OpenSearch.</br></p>

<p>3. Create an AWS Lambda Function</br>

Go to AWS Lambda > Create function.</br>

Choose Author from scratch.</br>

Provide a Function Name (e.g., rag-bedrock-lambda).</br>

Select Python 3.9+ as the runtime.</br>

Choose an existing IAM role or create a new one with:</br>

AWSBedrockFullAccess</br>

AWSLambdaBasicExecutionRole</br>

AmazonOpenSearchServiceFullAccess</br>

Click Create function.</br>

Write the function using lambda.py.</br></p>

<p>4. Configure Environment Variables</br>

Inside your Lambda function, navigate to the Configuration tab.</br>

Click Environment variables > Edit.</br>

Add:</br>

KNOWLEDGE_BASE_ID = {your-knowledge-base-id}</br>

FOUNDATION_MODEL_ARN = {Amazon Titan v2 ARN}</br>

To get the Foundation Model ARN, run:</br>

aws bedrock list-foundation-models<br/></p>

<p>5. Increase Lambda Timeout</br>

Update the Lambda function timeout to at least 1 minute (default is 3 seconds, which is insufficient).</br></p>

<p>6. Test the Lambda Function</br>

Click Test inside Lambda.</br>

Use this JSON payload:</br>

{ "user_query": "What is Amazon Bedrock?" }<br/>

Click Invoke and check the response.</br></p>

<p>7. Create an API Gateway and Deploy</br>

Navigate to API Gateway.</br>

Click Create API > HTTP API.</br>

Choose Add Integration > Lambda Function.</br>

Select the Lambda function.</br>

Click Next, review, and Create API.</br>

Click on Stages > Create Stage.</br>

Name it (your preference).</br>

Deploy and note down the Invoke URL.</br></p>

<p>8. Test API Using Postman</br>

Open Postman.</br>

Select POST request.</br>

Enter {Invoke URL} in the address bar.</br>

Set Headers:Content-Type: application/json</br>

Enter Body:{ "user-query": "What is Amazon Bedrock?" }<br/>

Click Send a POST request with a question in the request body.</br>

Receive and verify the response.</br></p>

<p>9. To create Streamlit App</br>
  pip install streamlit</br>
  use app.py to write the code</br>
  to run the python code "streamlit run app.py"</p></br>
    
<p>10. Test the Web Application</br>
   Open the designated port in a browser.</br>
   Enter a question and verify the response.</br></p>

    <h2>Conclusion</h2>
    <p>This project integrates Amazon Bedrock with OpenSearch to enable document-based Q&A via AI-powered embeddings and retrieval.</p>


