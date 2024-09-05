# Lambda

- 15 min runtime

## Lambda@Edge

Maximum size of 1 MB.

Limitations:

- does not support environment variables
- must be in us-east-1
- cannot be in VPC
- cannot be referenced with $LATEST$ or aliases; must use a version number
- can only be triggered by certain CloudFront events
    - Viewer request
    - Origin request
    - Origin response
    - Viewer response
