# Automate AWS EC2

---

## Challenge.....

### 1. Every Hour check your instance.

### 2. If any are stopped, you need to start them.

---


Create a lambda function:

<img width="1099" alt="Screenshot 2025-05-03 at 12 16 11 AM" src="https://github.com/user-attachments/assets/0916fab5-a497-4adf-92b8-c2e41f536636" />

Allow EC2 Permisiions:

<img width="1382" alt="Screenshot 2025-05-03 at 12 19 13 AM" src="https://github.com/user-attachments/assets/f147f083-e6e7-4908-a000-efd804ef9928" />


Writing the code:

<img width="1137" alt="Screenshot 2025-05-03 at 12 36 11 AM" src="https://github.com/user-attachments/assets/8312a270-4b75-418a-a75e-d7cc69259cd2" />

First Execution Failed: Because we don't have any EC2 instance.

<img width="801" alt="Screenshot 2025-05-03 at 12 36 42 AM" src="https://github.com/user-attachments/assets/1a48bab1-8da4-46b6-ae29-31808ec0ac41" />

So create some EC2 instance maybe 3 or 5. and also edit timeout setting.

<img width="1113" alt="Screenshot 2025-05-03 at 12 37 50 AM" src="https://github.com/user-attachments/assets/38438e31-3ded-4e49-bcf5-079ba4cf10ca" />

After creating Some EC2 instances let's test it again.

<img width="1108" alt="Screenshot 2025-05-03 at 12 38 50 AM" src="https://github.com/user-attachments/assets/d6bb780d-6c54-4834-a0ba-8af4428b4b19" />

It worked.....

<img width="1115" alt="Screenshot 2025-05-03 at 12 46 56 AM" src="https://github.com/user-attachments/assets/bfa7f3a7-df2a-4f0c-aac0-b1f4cb86b2b8" />

Now add a trigger for checking EC2 instance in every 60 minutes.

<img width="919" alt="Screenshot 2025-05-03 at 12 51 26 AM" src="https://github.com/user-attachments/assets/8db4c7e0-a6d7-4cf8-83c0-1d331f0786f3" />

---

