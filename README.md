# @nghianhdev/chat-review

## Install

```sh
npm i @nghianhdev/chat-review
```

## Usage

### Node.js

```js
import review from '@nghianhdev/chat-review';

review({
  gitlabConfig: {
    host: 'https://gitlab.mokahr.com/',
    mrIId: 2001,
    projectId: 200,
    token: 'glpat-xxxxxx',
  },
  chatgptConfig: {
    apiKey: 'sk-xxxxxxxxx',
  },
});
```

### Shell

```sh
chat-review --chatgpt sk-xxxxxxxxx --token 'glpat-xxxxxx' --project 200 --mr 2001
```

ChatGPT is a core CLI tool, including the following instructions:

- `--chatgpt`: API Token of ChatGPT.
- `--token`: GitLab access token.
- `--project`: GitLab project ID.
- `--mr`: GitLab Merge Request ID.
- `--model`: ChatGPT model type, default is `gpt-3.5-turbo`.
- `--language`: The language type of ChatGPT, the default is Chinese.
- `--host`: GitLab access address, the default is `https://gitlab.com`.
- `--proxyHost`: ChatGPT API host, the default is `https://api.openai.com`.
- `--target`: GitLab Review file, default is /\.(j|t)sx?$/

### CI

In Gitlab CI/CD configuration CHATGPT_KEY, GITLAB_TOKEN change, `.gitlab-ci.yml` as below:

```yml
stages:
  - merge-request

Code Review:
  stage: merge-request
  image: node:latest
  script:
    - npm i @nghianhdev/chat-review -g
    - echo "$CI_MERGE_REQUEST_PROJECT_ID" 
    - echo "$CI_MERGE_REQUEST_IID"
    - chat-review run --chatgpt "$CHATGPT_KEY" --token "$GITLAB_TOKEN" --project "$CI_MERGE_REQUEST_PROJECT_ID" --mr "$CI_MERGE_REQUEST_IID"
  only:
    - merge_requests
  except:
    variables:
      - $CI_MERGE_REQUEST_TARGET_BRANCH_NAME !~ /^(main|release)$/
  when: manual
```
