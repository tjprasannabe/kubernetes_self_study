# Developing Network policy

- Our goal is
    - to allow only `API` to `DB:3306` traffic
    - block all other traffic to DB
    - web and API pod all traffics are fine
    
    ![Untitled](Developing%20Network%20policy%2080d07fa08c8c44c9b66fcc8c468df4a7/Untitled.png)
    
    What we do 
    
- since k8 allow all pods communication
    - we need to block all the in/out communication at DB pod

> **Note:** if we define ingress policy, egress will be allowed automatically for that ingress traffic
> 

### Ingress policy:

- below policy allows ingress traffic from API pod to DB host port 3306 (and allows egress by default)
- all other traffic is blocked for DBhost

![Untitled](Developing%20Network%20policy%2080d07fa08c8c44c9b66fcc8c468df4a7/Untitled%201.png)

Use cases:

- podSelector
- nameSpaceSelector
- ipBlocks

### Case: 1

- if we have multiple API pod in different namespace  then
- Below allows from only the namespace : prod

![Untitled](Developing%20Network%20policy%2080d07fa08c8c44c9b66fcc8c468df4a7/Untitled%202.png)

### Case: 2

- what if we dont have `podSelector` defined
- only the `namespaceselector` defined?
    - it allows all pods to communicate into the DB:3306 from prod namespace

![Untitled](Developing%20Network%20policy%2080d07fa08c8c44c9b66fcc8c468df4a7/Untitled%203.png)

### Case: 3

- what of we have a backup server outside of cluster,
- then we can define ipBlocks to allow from specific IP addresses

![Untitled](Developing%20Network%20policy%2080d07fa08c8c44c9b66fcc8c468df4a7/Untitled%204.png)

### Case: 4

- If we have both specified, then both of the traffics are allowed
- Podselector AND namespaceselector —> both has to be met - then ingress allowed
- (Podselector AND namespaceselector ) OR ipBlock - here OR condition

![Untitled](Developing%20Network%20policy%2080d07fa08c8c44c9b66fcc8c468df4a7/Untitled%205.png)

### Case: 5

- here we added “-” hyphen for namespaceselector
    - this will be considered as separate rule

![Untitled](Developing%20Network%20policy%2080d07fa08c8c44c9b66fcc8c468df4a7/Untitled%206.png)

### Egress:

- if we have agent in DB , that pushes to Backup server (egress from DB to Backup)

![Untitled](Developing%20Network%20policy%2080d07fa08c8c44c9b66fcc8c468df4a7/Untitled%207.png)
