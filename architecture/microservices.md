# Microservices

### Definition

__Small__ and __autonomous__ services that work together.

#### Small
- focus on doing one thing well 
- how small? - ex: if code no longer feels too big

#### Autonomous
- isolate service, ex: docker image
- able to change and deployed independently
- no shared libraries and databases between services
- communication only via network calls (ex: REST)

## Benefits
- different teams
- different technology stack
- scale up only service services that requires more resources
- resilience:
    - one component down but not entire system 
- easier deployment
- easier to replace in the future


# Architect
- strategic goals
- principles (rules)
- practices (practical guidance)

1. Standard
    - how the service should look like
    - constant for every service

2. Monitoring
    - logging
    - metrics
    - health

3. Interfaces (API)
    - common approach across services
    - versioning
    - REST: verbs or nouns, plural or singular
    - pagination

4. Safety
    - how to handle the potential failure of downstream calls

5. Guidance
    - written documentation
    - examples:
        - should be real world services that get things right
    - service template
        - optional, easy to use, small
    
6. Technical debt
    - if we decide to cut a few corners to get some urgent features out -
if might have a short term benefit but a long-term cost
    - look at the bigger picture

7. Exception handling 