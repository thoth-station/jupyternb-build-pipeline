# jupyternb-build-pipeline

OpenShift-pipeline and TektonCD based pipeline for packaging jupyter notebooks into image for AICoE.

A pipeline that packages Jupyter notebooks into a container image. Container images can be circulated with others in a more convenient way.<br>
The final image will be uploaded to [aicoe-notebooks](https://registry-console.upshift.redhat.com/registry#/images/aicoe-notebooks) repo after the pipeline is finished.<br>
This registry is Red Hat internal and all images in aicoe-notebooks repo are public by default.

## Required repository Structure:

This pipeline looks for a "notebooks" directory in the root of your repo and copies the contents to the Jupyter home directory (/opt/app-root/src/).<br>
The dependencies for notebooks should be managed using a Pipfile and a Pipfile.lock, and these dependencies will be installed during the build time.

We have a template repo which you can use to structure your repo.

## Pipeline Setup:

On the GitLab/GitHub repo include the following webhook and also include sesheta as a collaborator.

Webhook: <http://thoth-ci.thoth.ultrahook.com><br>
Webhook Secret: `*******`(contact thoth-station)

Gitlab private/personal repository requirement: Add @sesheta as a collaborator in the project.

## Pipeline Details:

The pipeline listens to Tag release events in the repository it is linked to.<br>
On each Tag release, it will trigger a new build and push the updated image to the upshift registry based on the name of the repository.

**Note**: The name of the repository is to be lowercase.

The pipeline uses an s2i-minimal-notebook(Python3.6) image as the base image for s2i build.<br>
On image build, it pushes the tag to image-registry and also updates the latest tag for the image on the registry.

## How to contribute

We welcome contributions, The following components can be worked on:

- Tasks:

  - All new tasks are to be added in the tasks directory.
  - Make sure to add new resource required for the tasks in the resource.yaml

- Pipeline:

  - New pipeline are to be added in the pipeline directory.

- Events:

  - Please create new events for eventlistener, along with the triggertemplate and triggerbindings.

## Want to step up an instance

- Setup Tekton Pipeline and Tekton Trigger in cluster.
- Deploy the Triggers, Pipeline, Tasks.
- If behind the VPN, one time setup components:

  - Ultrahook: ultrahook passes the public internet request to services behind VPN

    - ultrahook secret and deployment with destination as jupyternb listener
