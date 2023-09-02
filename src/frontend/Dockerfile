# Copyright 2020 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Stage 1: Build the Go Application
FROM golang:1.15-alpine as builder

# Install required tools and dependencies
RUN apk update && apk add --no-cache ca-certificates git

# Set the working directory
WORKDIR /src

# Copy go.mod and go.sum files to download dependencies
COPY go.mod go.sum ./

# Download Go module dependencies
RUN go mod download

# Copy the rest of the application source code
COPY . .

# Build the Go application
RUN go build -o /go/bin/frontend .

# Stage 2: Create the Release Image
FROM alpine as release

# Install required tools and dependencies
RUN apk update && apk add --no-cache ca-certificates busybox-extras net-tools bind-tools

# Set the working directory for the release image
WORKDIR /frontend

# Copy the compiled Go binary from the builder stage
COPY --from=builder /go/bin/frontend /frontend/server

# Copy application templates and static files
COPY ./templates ./templates
COPY ./static ./static

# Expose the application's listening port
EXPOSE 8080

# Define the entry point for the container
ENTRYPOINT ["/frontend/server"]
