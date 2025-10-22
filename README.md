Follow the videos at https://youtu.be/2fXNkhltmNQ?si=ZNVGsp4g_re3mpAY and https://youtu.be/vfuhwB4Gmac?si=NvQJWZJDK4QtFm-0

Following are the changes made to read me add your own client ID and Client Secret: 
# Application Configuration for Dynamic OAuth2 Registration
server:
  port: 8080

spring:
  application:
    name: spring-cloud-gateway-dynamic-reg
  security:
    oauth2:
      client:
        registration:
          google:
            **client-id: 554872852571-dv96ht7qfhqj6fkk4o773kra1433p7dn.apps.googleusercontent.com 
            client-secret: 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 **
            authorization-grant-type: authorization_code
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
            scope: 
              - openid
              - email
              - profile
        provider:
          google:
            authorization-uri: https://accounts.google.com/o/oauth2/v2/auth
            token-uri: https://oauth2.googleapis.com/token
            user-info-uri: https://www.googleapis.com/oauth2/v3/userinfo
            user-name-attribute: sub
  cloud:
    gateway:
      routes:
        - id: student-service
          uri: http://localhost:8090
          predicates:
            - Path=/student/**
          filters:
            - StripPrefix=1
            - name: TokenRelay
              args:
                client-registration-id: "google"

# Security debugging (reduce noise after initial setup)
logging:
  level:
    org.springframework.cloud.gateway: DEBUG
    org.springframework.security.oauth2.client: TRACE
    org.springframework.security: WARN
    org.springframework.web: WARN
