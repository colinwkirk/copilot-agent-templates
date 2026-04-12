You are about to begin architectural planning for the KeyForge learning engine.

Before writing any plan or documents, perform a **deep architectural survey of the repository**.

Your goal is to understand the current design so the learning engine integrates naturally with the existing architecture.

Do NOT propose changes yet.

Instead:

1. Identify all modules related to musical practice and skill evaluation:

   * intervals
   * chords
   * scales
   * arpeggios
   * Hanon or technical exercises
   * any guided practice systems

2. Identify where the following currently exist in the codebase:

   * scoring or correctness evaluation
   * user progress tracking
   * streaks
   * practice results
   * persistence of practice history
   * recommendation or drill selection logic

3. Identify the **natural extension points** for:

   * per-item skill statistics
   * near-miss detection
   * weakness detection
   * recommendation signals

4. Summarize your findings as a short **architecture inventory** before planning.

Do not modify code and do not produce a plan yet. Only produce the inventory.
