# Architecting and Ecosystem

## Guiding Principes

- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures (design based on changing requirements)
- Drive architecture using data
- Improve through game days (simulate application for flash sale days)

## AWS Cloud Best Practices - Design Principles

**Scalability**: vertical & horizontal
**Disposable Resources**: servers should be disposable & easily configured
**Automation**: Serverless, IaaS, Auto Scalling...
**Loose Coupling**
**Services not Servers**

## Well Architected Framework 6 Pillars
1. Operational Excellence = includes ability to run and monitor systems to deliver business value and continually improve supporting processes and procedures with the following design principles:
- Perform operations as code: IaaS
- Annotate documentation: automate creation of annotated documentation after each build
- Make frequent, small, reversible changes
- Refine operation procedures frequently
- Anticipate failure
- Learn from all operational failures

| Prepare | Operate | Evolve |
| ------- | ------- | ------ |
| Cloud Formation | Cloud Formation | Cloud Formation| 
| Config | Config | Code Build| 
| | CloudTrail | Code Commit |
| | CloudWatch | CodeDeploy |
| | AWS X-Ray | CodePipeline |

3. Security
4. Reliability
5. Performance Efficiency
6. Cost Optimization
7. Sustainability

The'ye a synergy (not to balance or trade-offs)
