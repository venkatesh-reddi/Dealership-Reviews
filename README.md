# Dealerships Review Website

## Overview
The Dealerships Website is a comprehensive web application designed to manage car dealerships and customer reviews. This project demonstrates a full-stack implementation using Django for the web application, Node.js for backend services, MongoDB for data management, and additional cloud services for sentiment analysis.

## Architecture Overview
The project is designed with a modular architecture, incorporating various technologies to deliver a seamless user experience. The solution consists of the following components:

1. **Django Web Application**: Serves the main website and provides user management, static pages, and dynamic pages for dealers and reviews.
2. **Express Mongo Service**: Manages dealers and reviews, running in a Docker container.
3. **Sentiment Analyzer Service**: Deployed on IBM Cloud Code Engine, analyzes the sentiment of review texts.
4. **SQLite Database**: Stores car make and car model data.
5. **Django Proxy Service**: Integrates the Django application with the Express Mongo service and the Sentiment Analyzer service.

![Architecture Overview Diagram](https://github.com/venkatesh-reddi/Dealership-Reviews/blob/main/frontend/static/dealership-architecture.png)

## Project Breakdown

### 1. Home Page, Contact Page, and About Page
- **Home Page**: Provides an overview of the dealership's offerings.
- **Contact Page**: Allows users to get in touch with the dealership.
- **About Page**: Shares the dealership's history and mission.

### 2. User Management Overview
To manage users and their access based on roles such as anonymous users or registered users, the following capabilities can be implemented:

- **Login**: Handle user login requests.
- **Logout**: Handle user logout requests.
- **Registration**: Handle user sign-up requests.

### 3. Backend Services (Node.js & Express App)
The backend services are managed by a Node.js application using Express, containerized with Docker. These services provide API endpoints for dealership and review management, accessible through the following endpoints:

- `/fetchReviews/dealer/29`
- `/fetchDealers`
- `/fetchDealer/3`
- `/fetchDealers/Kansas`

### 4. Django Models and Views
To manage the dealers' inventory and integrate external data, the following data models and services are used:

- **CarModel and CarMake Django Models**: Define models for car makes and models.
- **Admin and Registered User Manipulation**: Admins can create, read, update, and delete car models and makes via the Django admin interface. Registered users can view and interact with these models through the web application.

### 5. Dynamic Pages Overview
To present service results to end users using stylized front-end React pages:

- **Data Fetching**: Use Django views and proxy services to fetch data from external resources based on user requests.
- **Data Presentation**: Render the fetched data using React components on the frontend.

### 6. Continuous Integration and Continuous Delivery (CI/CD) Overview
To streamline collaboration and delivery of changes, CI/CD is set up using GitHub workflows:

- **GitHub Workflows**: Configure CI/CD pipelines for automated testing and linting.
- **Linting and GitHub Actions**: Enable GitHub Actions to run the linting workflow and ensure code quality.

### 7. Containerize & Deploy to IBM Cloud
To leverage container management and avoid vendor lock-in, the application is containerized and deployed using Kubernetes:

- **Container Implementation**: Package the application with environment variables, configuration files, libraries, and dependencies.
- **IBM Cloud Deployment**: Deploy the containerized application to IBM Cloud using Kubernetes for orchestration.

## Solution Architecture

### Web Application (Django)
The Django application serves as the frontend, providing the following endpoints:
- `get_cars/`: Retrieve the list of cars.
- `get_dealers/`: Retrieve the list of dealers.
- `get_dealers/:state`: Retrieve dealers by state.
- `dealer/:id`: Retrieve dealer details by ID.
- `review/dealer/:id`: Retrieve reviews for a specific dealer.
- `add_review/`: Post a review about a dealer.

### Backend Services (Express Mongo Service)
Running in a Docker container, this service provides:
- `/fetchDealers`: Fetch all dealers.
- `/fetchDealer/:id`: Fetch dealer details by ID.
- `/fetchReviews`: Fetch all reviews.
- `/fetchReview/dealer/:id`: Fetch reviews for a specific dealer.
- `/insertReview`: Insert a new review.

### Sentiment Analyzer (IBM Cloud Code Engine)
Provides sentiment analysis for review texts:
- `/analyze/:text`: Analyze the sentiment of the provided text, returning positive, negative, or neutral.

### Database
- **SQLite**: Used by the Django application to store car make and car model data.
- **MongoDB**: Used by the Express service to store dealers and reviews.

## Installation Instructions

### Clone the Repository
```sh
git clone https://github.com/venkatesh-reddi/Dealership-Reviews.git
cd Dealership-Reviews
```

### Set Up the Django Application
1. **Create and activate a virtual environment**
   ```sh
   pip install virtualenv
   virtualenv djangoenv
   source djangoenv/bin/activate
   ```

2. **Install dependencies**
   ```sh
   python3 -m pip install -U -r requirements.txt
   ```

3. **Apply database migrations**
   ```sh
   python3 manage.py makemigrations
   python3 manage.py migrate
   ```

4. **Start the Django server**
   ```sh
   python3 manage.py runserver
   ```

### Set Up the React Frontend
1. **Open a new terminal and navigate to the frontend directory**
   ```sh
   cd Dealership-Reviews/frontend
   ```

2. **Install dependencies and build the project**
   ```sh
   npm install
   npm run build
   ```

### Set Up the Backend Express MongoDB Server
1. **Open a new terminal and navigate to the database directory**
   ```sh
   cd Dealership-Reviews/database
   ```

2. **Build the Docker image and start the server**
   ```sh
   docker build . -t nodeapp
   docker-compose up
   ```

### Deploy Sentiment Analyzer to IBM Cloud Code Engine
1. **Open a new terminal and navigate to the microservices directory**
   ```sh
   cd Dealership-Reviews/djangoapp/microservices
   ```

2. **Build the Docker image**
   ```sh
   docker build . -t <your-docker-repo>/senti_analyzer
   ```

3. **Push the Docker image to your Docker repository**
   ```sh
   docker push <your-docker-repo>/senti_analyzer
   ```

4. **Deploy the application to IBM Cloud Code Engine**
   ```sh
   ibmcloud ce application create --name sentianalyzer --image <your-docker-repo>/senti_analyzer --registry-secret icr-secret --port 5000
   ```

### Update Django Application Configuration
1. **Add the following URLs to the `djangoapp/.env` file**
   ```env
   backend_url=your_backend_url_of_express_mongodb_server
   sentiment_analyzer_url=url_of_senti_analyzer_deployment_on_ibmcloud
   ```
## Usage Guide

### Django Application

1. **Access the Admin Panel**
   - Open your web browser and go to `http://localhost:8000/admin/`
   - Log in using the superuser credentials you created during setup.
   - Here, you can manage users, car models, car makes, and other backend data.

2. **View the Website**
   - Open your web browser and go to `http://localhost:8000/`
   - You will see the home page of the Dealership-Reviews website.

3. **Explore the Static Pages**
   - Navigate to the "About Us" page by clicking on the "About Us" link in the navigation bar.
   - Navigate to the "Contact Us" page by clicking on the "Contact Us" link in the navigation bar.

### React Frontend

1. **Access the React Frontend**
   - Open your web browser and go to `http://localhost:3030/`
   - You will see the React frontend of the Dealership-Reviews website.

2. **Explore Dynamic Pages**
   - Click on the "Dealers" link to view all dealerships.
   - Click on a specific dealer to view their details and reviews.
   - Use the review submission page to add a review for a dealer.

### Backend Express MongoDB Server

1. **API Endpoints**
   - The backend server provides the following API endpoints:
     - `/fetchReviews/dealer/29` - Fetch reviews for a specific dealer.
     - `/fetchDealers` - Fetch all dealerships.
     - `/fetchDealer/3` - Fetch a specific dealer by ID.
     - `/fetchDealers/Kansas` - Fetch dealerships in Kansas.

### Sentiment Analyzer Service

1. **Sentiment Analysis**
   - The sentiment analyzer service analyzes text passed to `/analyze/:text` endpoint.
   - It returns whether the sentiment is positive, negative, or neutral.

### Continuous Integration and Deployment

1. **GitHub Actions**
   - GitHub Actions are set up for continuous integration and linting.
   - Any changes pushed to the repository trigger linting checks automatically.

2. **Containerization and Deployment**
   - The application can be containerized using Docker.
   - Push the Docker images to your Docker repository.
   - Deploy the application to IBM Cloud Code Engine using the provided deployment script.

## Contribution

Feel free to contribute to this project by opening issues and submitting pull requests. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the Apache 2.0 License.

---

Happy coding! If you have any questions or need further assistance, please don't hesitate to reach out.

---

Venkatesh Reddi

[GitHub Profile](https://github.com/venkatesh-reddi) | [LinkedIn Profile](https://linkedin.com/in/venkateshreddi)
