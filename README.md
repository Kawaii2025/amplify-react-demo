# AWS Amplify React Demo

A full-stack demo app with React frontend on AWS Amplify and Spring Boot backend on Elastic Beanstalk.

## Features

- ✅ Auto-deploy on Git push via AWS Amplify
- ✅ API connection to Spring Boot backend
- ✅ CORS configured for separate deployments
- ✅ Health check monitoring
- ✅ Modern UI with Tailwind CSS

## Local Development

1. Install dependencies:
```bash
npm install
```

2. Start development server:
```bash
npm start
```

3. Make sure your Spring Boot backend is running on `http://localhost:8080`

## Deploy to AWS Amplify

### Method 1: Amplify Console (Easiest)

1. Push code to GitHub
2. Go to [AWS Amplify Console](https://console.aws.amazon.com/amplify)
3. Click "New app" → "Host web app"
4. Connect your GitHub repository
5. Select this repository and branch (main)
6. Amplify auto-detects settings from `amplify.yml`
7. Add environment variable:
   - Key: `REACT_APP_API_URL`
   - Value: `https://your-backend-url.elasticbeanstalk.com`
8. Click "Save and deploy"

### Method 2: Amplify CLI
```bash
# Install Amplify CLI
npm install -g @aws-amplify/cli

# Configure
amplify configure

# Initialize
amplify init

# Add hosting
amplify add hosting

# Publish
amplify publish
```

## Environment Variables

- `REACT_APP_API_URL`: Your Spring Boot backend URL

## Spring Boot Backend Setup

Your Spring Boot backend needs CORS configuration:
```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins(
                    "http://localhost:3000",
                    "https://your-amplify-app.amplifyapp.com"
                )
                .allowedMethods("GET", "POST", "PUT", "DELETE");
    }
}
```

## Architecture
```
GitHub Push → AWS Amplify (Auto Build & Deploy)
                    ↓
            React App on CloudFront
                    ↓ API Calls
        Spring Boot on Elastic Beanstalk
```

## License

MIT
