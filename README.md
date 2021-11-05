# Links
- Source Repo: https://github.ibm.com/cloud-docs-allowlist/databases-for-mysql

# Branches:
- draft > publishes to https://test.cloud.ibm.com/docs/allowlist/databases-for-mysql
- publish > publishes to https://cloud.ibm.com/docs/allowlist/databases-for-mysql
- Output: After you commit and build to test: https://test.cloud.ibm.com/docs/allowlist/databases-for-mysql

# Managing access to your repo
- You are the admin on the repo; at any time you can add others and provide them with the access level you want them to have.

# To add collaborators:
- Navigate to Settings > Collaborators and Teams > Collaborators
- Add the collaborator GitHub Enterprise ID, and click Add collaborator
- Specify the the permission level as Admin if you want them to have full permission to author directly to the repo. Specify Write permission to give access to read, and push a Pull Request up to your repo.
- Monitoring your builds
- Every build comes with a Slack channel that you can use to monitor and click through to view the builds.

# Your Slack build monitoring channel: #docs-databases-for-mysql

# Need access to Jenkins? 
https://test.cloud.ibm.com/docs/developing/writing?topic=writing-trigger_monitor#auth-access

# Next steps
- Start making changes to your draft branch. In your toc file, define your test global catalog ID value on the allowlist-id attribute. This value is required to build and view your docs. See the docs for more information: https://test.cloud.ibm.com/docs/writing?topic=writing-allowlisting

- When you're ready to go to production, move your files to the publish branch. Make sure your toc file contains the production catalog entry ID on the allowlist-id attribute. When you commit the files to publish and subsequent commits, your source from the publish branch automatically build to your subcollection on https://cloud.ibm.com/docs/allowlist
