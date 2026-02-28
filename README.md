# DevOps Assignment

This project consists of a FastAPI backend and a Next.js frontend that communicates with the backend, with infrastructure managed via Terraform on AWS.

## Project Structure

```
.
├── backend/               # FastAPI backend
│   ├── app/
│   │   └── main.py       # Main FastAPI application
│   └── requirements.txt    # Python dependencies
├── frontend/              # Next.js frontend
│   ├── pages/
│   │   └── index.js     # Main page
│   ├── public/            # Static files
│   └── package.json       # Node.js dependencies
└── terraform/             # Infrastructure as Code
    └── environments/
        └── dev/           # Development environment
            ├── modules/   # Reusable Terraform modules (VPC, EC2, IAM, SG)
            ├── main.tf    # Main infrastructure definition
            ├── backend.tf # Remote S3 + DynamoDB configuration
            └── terraform.tfvars # Environment variables
```

## Infrastructure (Terraform)

The infrastructure is provisioned on AWS using Terraform with a secure remote backend.

### Features
- **Remote Backend**: State is stored in S3 (`terraform-state-nandhushree-a`) with versioning enabled.
- **State Locking**: DynamoDB (`terraform-lock-table`) is used to prevent concurrent state modifications.
- **Modular Design**: Reusable modules for VPC, EC2, IAM, and Security Groups.
- **Security**: EC2 is provisioned in a custom VPC with specific Security Group rules (SSH: 22, HTTP: 80).

### Deployment Steps
1. Navigate to the terraform directory:
   ```bash
   cd terraform/environments/dev
   ```
2. Initialize Terraform:
   ```bash
   terraform init
   ```
3. Plan and Apply:
   ```bash
   terraform plan
   terraform apply
   ```

## Backend Setup (FastAPI)

1. Navigate to the backend directory:
   ```bash
   cd backend
   ```

2. Create a virtual environment (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: .\venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Run the FastAPI server:
   ```bash
   uvicorn app.main:app --reload --port 8000
   ```

   The backend will be available at `http://localhost:8000`

## Frontend Setup (Next.js)

1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   # or
   yarn
   ```

3. Configure the backend URL:
   - Open `.env.local`
   - Update `NEXT_PUBLIC_API_URL` with your backend URL (e.g., EC2 Public IP)
   - Example: `NEXT_PUBLIC_API_URL=http://13.233.98.29:8000`

4. Run the development server:
   ```bash
   npm run dev
   ```

   The frontend will be available at `http://localhost:3000`

## Deployment

For production deployment of the frontend:
```bash
npm run build
npm run start
```
