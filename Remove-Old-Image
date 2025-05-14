#!/bin/bash

DOCKER_USER="rohit5521"
IMAGE_NAME="myimage"
KEEP=5  # Number of recent images to keep

# Get all image tags sorted by creation date (newest first)
TAGS=$(docker images --format "{{.Tag}}\t{{.CreatedAt}}" "${DOCKER_USER}/${IMAGE_NAME}" | \
       sort -k2 -r | \
       awk '{print $1}')

# Count total versions
TOTAL=$(echo "$TAGS" | wc -l)

if [ "$TOTAL" -le "$KEEP" ]; then
    echo "No cleanup needed - only $TOTAL images exist (keeping $KEEP)"
    exit 0
fi

echo "Found $TOTAL images. Keeping the $KEEP most recent:"

# Get images to keep (most recent + 'latest' if exists)
KEEP_TAGS=$(echo "$TAGS" | head -n $KEEP)
echo "Keeping these tags:"
echo "$KEEP_TAGS" | sed 's/^/- /'

# Get images to delete (all except kept ones)
DELETE_TAGS=$(echo "$TAGS" | tail -n +$((KEEP+1)))
if [ -z "$DELETE_TAGS" ]; then
    echo "No old images to delete"
    exit 0
fi

echo "Deleting these old tags:"
echo "$DELETE_TAGS" | sed 's/^/- /'

# Perform deletion
for TAG in $DELETE_TAGS; do
    echo "Deleting ${DOCKER_USER}/${IMAGE_NAME}:${TAG}"
    docker rmi "${DOCKER_USER}/${IMAGE_NAME}:${TAG}"
done

echo "Cleanup completed. Current images:"
docker images "${DOCKER_USER}/${IMAGE_NAME}"
