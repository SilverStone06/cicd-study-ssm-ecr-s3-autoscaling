# git-cicd-study-ssm-ecr-s3-auto-scaling-5

Nginx 이미지를 ECR에 푸시하고, S3에서 `nginx-private/` 디렉터리를 내려받아 이미지를 빌드한 뒤, Auto Scaling 대상 EC2에 SSM으로 배포하는 파이프라인 리포지토리입니다.

## 워크플로우
- S3 `s3://<bucket>/nginx-private/`에서 파일 다운로드
- ECR 로그인 → 이미지 빌드/푸시
- SSM으로 태그 대상 인스턴스에 배포

## 필요 시크릿
- `AWS_ACCESS_KEY_ID`: 배포용 IAM 사용자의 액세스 키 ID
- `AWS_SECRET_ACCESS_KEY`: 배포용 IAM 사용자의 시크릿 액세스 키
- `AWS_REGION`: 리소스가 있는 AWS 리전
- `S3_BUCKET_NAME`: S3 버킷 이름
- `ECR_REPOSITORY`: Nginx 이미지용 ECR 리포지토리 이름
- `ASG_TAG_KEY`: SSM 대상 인스턴스를 찾는 태그 키
- `EC2_INSTANCE_TAG`: SSM 대상 인스턴스를 찾는 태그 값

## 참고
- SSM 대상은 `tag:<ASG_TAG_KEY>=<EC2_INSTANCE_TAG>` 조건으로 선택됩니다.
- 배포 시 EC2에 `/home/ubuntu/app` 경로를 사용합니다.\n