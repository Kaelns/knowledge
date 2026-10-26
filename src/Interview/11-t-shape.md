# 11 · T-shape: cross-mobile, cloud, hosting, Gen AI

Matrix (A2): **JS Cross-Mobile Platform: Intermediate** (new, required) · Gen AI Assisted Development: Intermediate *(optional)* · Cloud Fundamentals & Web App Hosting: *not applicable* for A2, but basic awareness is cheap and makes answers about deployment stronger.
Phase 2: Wed Nov 4 (mobile) and Thu Nov 5 (cloud, hosting, AI).

## Cross-mobile (A2 Intermediate)

- [ ] Landscape: **React Native** (native components via JS, New Architecture: JSI/Fabric/TurboModules) vs **Expo** (managed RN tooling) vs Ionic/Capacitor (web in a WebView) vs Cordova/PhoneGap (legacy) vs Flutter (Dart, non-JS) vs PWA
- [ ] RN basics: `View`, `Text`, `Image`, `ScrollView`/`FlatList`, `StyleSheet` + flexbox (column by default), `Pressable`, platform-specific code (`Platform.OS`, `.ios.tsx`), React Navigation
- [ ] Native look & feel: native components, safe areas, gestures/animations (Reanimated)
- [ ] Device features: geolocation, camera, accelerometer via Expo modules (`expo-location`, `expo-camera`, `expo-sensors`), permissions
- [ ] Architecture: same React patterns (state, React Query works as-is), REST API integration, offline storage (AsyncStorage/MMKV), secure storage for tokens
- [ ] Publishing: app signing, App Store Connect / TestFlight, Google Play Console, EAS Build/Submit, OTA updates (EAS Update), store review rules
- [ ] Performance: avoid JS-thread blocking, `FlatList` virtualization, memoization, Hermes engine, image optimization
- [ ] Hands-on (2h): `npx create-expo-app`, a list screen fetching an API with React Query + a details screen; run in Expo Go on my phone

Key questions: How is React Native different from React DOM? RN vs Ionic vs PWA? How do you access the camera? How do you publish an app?

Links: [React Native docs](https://reactnative.dev/docs/getting-started) · [Expo docs](https://docs.expo.dev/)

## Cloud & hosting (awareness)

- [ ] Service models: IaaS (EC2), PaaS (Elastic Beanstalk, App Service), SaaS, FaaS (Lambda)
- [ ] Compute: VMs vs containers (ECS/Kubernetes) vs serverless; storage: object (S3) vs block vs file; DBs: managed SQL (RDS) vs NoSQL (DynamoDB)
- [ ] Messaging: queues (SQS), pub/sub (SNS), event streaming (Kafka), API gateway
- [ ] Hosting a SPA: S3/Azure Blob + CloudFront/CDN, or Vercel/Netlify; SSL certs (Let's Encrypt/ACM), custom domain + DNS, SPA fallback routing, cache invalidation
- [ ] Load balancing, CDN edge caching, edge functions; IaC idea (Terraform, CloudFormation) and CI/CD → deploy

Links: [AWS Cloud Practitioner essentials (free)](https://aws.amazon.com/training/digital/aws-cloud-practitioner-essentials/)

## Gen AI assisted development (optional, A2 Intermediate)

I already use Claude Code, so prepare concrete examples.
- [ ] LLM basics: tokens, context window, temperature, hallucinations; inline completion vs chat vs agent/CLI modes
- [ ] Agentic loop: plan → tool use → execute → observe → self-correct; Plan/Think mode before execution
- [ ] Context management: what goes into context, project instruction files (**CLAUDE.md**, `.cursorrules`, `copilot-instructions.md`), why concise context improves quality
- [ ] **MCP**: what it is (standard protocol for connecting tools/data to agents), servers I've used (e.g. Figma, Context7 docs)
- [ ] Permissions & approvals: file system, terminal, sandboxing
- [ ] Use cases: codebase exploration, test generation, bug investigation, docs; **reviewing** AI output critically (security, correctness, over-engineering)
- [ ] Story for the interview: one task where AI saved real time + one where I rejected its suggestion and why

Links: [Claude Code docs](https://docs.claude.com/en/docs/claude-code/overview) · [MCP intro](https://modelcontextprotocol.io/introduction) · my notes [AI.md](../Ai/AI.md)
