<!DOCTYPE html>
<html>
<head>
    <title>RAG-Based Solution Using Amazon Bedrock</title>
    <style>
        body { font-family: Arial, sans-serif; line-height: 1.6; }
        h1, h2, h3 { color: #333; }
        code { background: #f4f4f4; padding: 2px 4px; border-radius: 4px; }
        pre { background: #f4f4f4; padding: 10px; border-radius: 5px; overflow-x: auto; }
    </style>
</head>
<body>
    <h1>RAG-Based Solution Using Amazon Bedrock</h1>
    <p>This guide provides step-by-step instructions to build a Retrieval-Augmented Generation (RAG) system using Amazon Bedrock, OpenSearch, and AWS Lambda.</p>

    <h2>Steps</h2>
    <h3>1. Create an S3 Bucket and Upload Documents</h3>
    <ol>
        <li>Navigate to Amazon S3 in the AWS Console.</li>
        <li>Click Create bucket.</li>
        <li>Provide a unique bucket name (e.g., rag-knowledge-base).</li>
        <li>Choose Region: us-east-1 (Required for Amazon Bedrock).</li>
        <li>Disable Block all public access (as needed).</li>
        <li>Click Create bucket.</li>
        <li>Inside the S3 bucket, click Upload.</li>
        <li>Select your PDF, DOCX, TXT, or JSON files.Click Upload.</li>
        <li>Note the S3 URI (e.g., s3://rag-knowledge-base/documents/).</li>
    </ol>

    <h3>2. Set Up Knowledge Base in Amazon Bedrock</h3>
    <ol>
        <li>Navigate to Amazon Bedrock.</li>
        <li>Ensure you are in the us-east-1 region.</li>
        <li>Click on Knowledge Base > Create a knowledge base.</li>
        <li>Enter a Name (e.g., rag-kb).</li>
        <li>In Data Source, select Amazon S3.</li>
        <li>Provide the S3 bucket URI where documents are stored.</li>
        <li>Choose Chunking Strategy (Default recommended).</li>
        <li>Choose Amazon Titan v2 Embedding Model for embeddings.</li>
        <li>Select OpenSearch as the vector store.</li>
        <li>Click Create.</li>
        <li>Once the knowledge base is created, click Sync Now.</li>
        <li>Wait for the synchronization to complete.</li>
        <li>The documents are now indexed and vectorized in OpenSearch.</li>
    </ol>

    <h3>3. Create an AWS Lambda Function</h3>
    <ol>
        <li>Go to AWS Lambda > Create function.</li>
        <li>Choose Author from scratch.</li>
        <li>Provide a Function Name (e.g., rag-bedrock-lambda).</li>
        <li>Select Python 3.9+ as the runtime.</li>
        <li>Choose an existing IAM role or create a new one with:
           <li>AWSBedrockFullAccess</li>
           <li>AWSLambdaBasicExecutionRole</li>
           <li>AmazonOpenSearchServiceFullAccess</li>
        </li>
        <li>Click Create function.</li>
        <li>write the function using lambda.py</li>
    </ol>

    <h3>4. Configure Environment Variables</h3>
    <ol>
        <li>Inside your Lambda function, navigate to the Configuration tab.</li>
        <li>Click Environment variables > Edit.</li>
        <li>Add:
           <ul>
              <li>KNOWLEDGE_BASE_ID = {your-knowledge-base-id}</li>
              <li> FOUNDATION_MODEL_ARN = {Amazon Titan v2 ARN}</li>
           </ul>
        </li>
        <li>To get the Foundation Model ARN, run:</li>
        <pre><code>aws bedrock list-foundation-models</code></pre>        
    </ol>

    <h3>5. Increase Lambda Timeout</h3>
    <p>Update the Lambda function timeout to at least 1 minute (default is 3 seconds, which is insufficient).</p>

    <h3>6. Test the Lambda Function</h3>
    <ol>
        <li>Click Test inside Lambda.</li>
        <li>Use this JSON payload:</li>
        <pre><code>{
  "user_query": "What is Amazon Bedrock?"
}</code></pre>
        <li>Click Invoke and check the response.</li>
    </ol>
    <h3>7.Create an API Gateway and deploy</h3>
    <ol>
       <li>Navigate to API Gateway.</li>
       <li>Click Create API > HTTP API.</li>
       <li>Choose Add Integration > Lambda Function.</li>
       <li>Select the Lambda function.</li>
       <li>Click Next, review, and Create API.</li>
       <li>Click on Stages > Create Stage.</li>
       <li>Name it(your preference)<li>
       <li>Deploy and note down the Invoke URL.</li>
    </ol>
    
    <h3>7. Test API Using Postman</h3>
    <ol>
    <li>Open Postman.</li>
    <li>Select POST request.</li>
    <li>Enter {Invoke URL} in the address bar.</li>
    <li>Set Headers:Content-Type: application/json</li>
    <li>Enter Body:</li>
    <pre><code>
Body:
{
    "user-query": "What is Amazon Bedrock?"
}</code></pre>
    <li> click on Send a POST request with a question in the request body.</li>
    <li>Receive and verify the response.</li>

    <h3>8. Create a Streamlit Web Frontend</h3>
    <pre><code>pip install streamlit requests</code></pre>
    <p>Create <code>app.py</code></p>
    
    <h3>9. Run the Streamlit App</h3>
    <pre><code>streamlit run app.py</code></pre>
    
    <h3>10. Test the Web Application</h3>
    <ol>
        <li>Open the designated port in a browser.</li>
        <li>Enter a question and verify the response.</li>
    </ol>

    <h2>Conclusion</h2>
    <p>This project integrates Amazon Bedrock with OpenSearch to enable document-based Q&A via AI-powered embeddings and retrieval.</p>
</body>
</html>

