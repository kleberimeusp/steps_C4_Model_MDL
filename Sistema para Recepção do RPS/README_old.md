# C4 Model: System Container Diagram for RPS Reception Workflow

## Containers:

1. **Nginx**:
   - **Purpose**: Provides secure entry to the system through TLS authentication.
   - **Role**: Validates and forwards authenticated requests to the WS container.

2. **WS (Web Service)**:
   - **Purpose**: Receives requests from Nginx after successful authentication.
   - **Role**: Acts as the primary entry point to process and forward requests for further validation.

3. **Proxy-Declaracao**:
   - **Purpose**: Validates the company's permission and signature for processing the RPS.
   - **Role**: Serves as a middle layer for additional security checks and data integrity.

4. **Declaracao-WS**:
   - **Purpose**: Saves the validated RPS into a queue for further processing or storage.
   - **Role**: Ensures the RPS is reliably persisted for future actions.

## Workflow Steps:

1. The user request is received by **Nginx**, which performs **authentication using TLS**.
2. If authenticated, the request is forwarded to **WS**, which **receives and processes the user request**.
3. WS sends the request to **Proxy-Declaracao**, where **company permissions and signature are validated**.
4. Once validated, the **RPS is saved to the queue** by the **Declaracao-WS** service.
