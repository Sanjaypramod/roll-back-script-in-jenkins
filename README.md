# roll-back-using-helm

Rollback setup using jenkins freestyle job
Execute Shell:

Add a build step and select “Execute shell”.
Use the following script to include the helm history command and specify the namespace:
sh
Copy code
#!/bin/bash

# Variables from Jenkins parameters
RELEASE_NAME=$RELEASE_NAME
REVISION_NUMBER=$REVISION_NUMBER
NAMESPACE=$NAMESPACE

# Function to log messages
log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1"
}

log "Starting rollback process..."

# Validate inputs
if [ -z "$RELEASE_NAME" ]; then
    log "Error: RELEASE_NAME is not specified."
    exit 1
fi

if [ -z "$REVISION_NUMBER" ]; then
    log "Error: REVISION_NUMBER is not specified."
    exit 1
fi

if [ -z "$NAMESPACE" ]; then
    log "Error: NAMESPACE is not specified."
    exit 1
fi

# Show helm history
log "Fetching Helm history for release $RELEASE_NAME in namespace $NAMESPACE..."
helm history $RELEASE_NAME -n $NAMESPACE

# Perform the rollback
log "Rolling back release $RELEASE_NAME to revision $REVISION_NUMBER in namespace $NAMESPACE..."
helm rollback $RELEASE_NAME $REVISION_NUMBER -n $NAMESPACE

if [ $? -eq 0 ]; then
    log "Rollback succeeded."
else
    log "Rollback failed."
    exit 1
fi
4. Post-Build Actions (Optional)
Email Notifications:
Configure email notifications to notify you of the rollback status.
In the “Post-build Actions” section, select “Editable Email Notification” (if using the Email Extension Plugin) and configure the recipient list, subject, and body of the email.
5. Trigger the Job Manually
Whenever you need to roll back a specific release, navigate to the Jenkins job and click “Build with Parameters”.
Enter the RELEASE_NAME, REVISION_NUMBER, and NAMESPACE in the provided fields and click “Build”.
Example Job Configuration
Parameters Configuration:

RELEASE_NAME: The Helm release you want to roll back.
REVISION_NUMBER: The specific revision number to which you want to roll back.
NAMESPACE: The namespace of the Helm release.
Shell Script in Build Step:

sh
Copy code
#!/bin/bash

# Variables from Jenkins parameters
RELEASE_NAME=$RELEASE_NAME
REVISION_NUMBER=$REVISION_NUMBER
NAMESPACE=$NAMESPACE

# Function to log messages
log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1"
}

log "Starting rollback process..."

# Validate inputs
if [ -z "$RELEASE_NAME" ]; then
    log "Error: RELEASE_NAME is not specified."
    exit 1
fi

if [ -z "$REVISION_NUMBER" ]; then
    log "Error: REVISION_NUMBER is not specified."
    exit 1
fi

if [ -z "$NAMESPACE" ]; then
    log "Error: NAMESPACE is not specified."
    exit 1
fi

# Show helm history
log "Fetching Helm history for release $RELEASE_NAME in namespace $NAMESPACE..."
helm history $RELEASE_NAME -n $NAMESPACE

# Perform the rollback
log "Rolling back release $RELEASE_NAME to revision $REVISION_NUMBER in namespace $NAMESPACE..."
helm rollback $RELEASE_NAME $REVISION_NUMBER -n $NAMESPACE

if [ $? -eq 0 ]; then
    log "Rollback succeeded."
else
    log "Rollback failed."
    exit 1
fi
This setup allows you to manually trigger rollbacks for any of the applications deployed in your cluster, view the history of a release, and specify the namespace. By providing the release name, revision number, and namespace as parameters, you can target specific deployments as needed.








