# CSCI 210 - Module 16: Map and CSV Intro (BST Query)
## Assignment Completion Notes

### 1. Overview
Module 16 focuses on implementing and utilizing a **Map Abstract Data Type (ADT)** backed by a **Binary Search Tree (BST)** to store, index, and query columnar CSV datasets (specifically the Top 5000 Songs dataset).

---

### 2. Diagnosis & Root Cause
When initially checking the project by running the test suite (`./gradlew test`), both test cases failed:
- `treesappTest > testQueryEltonJohn() FAILED` (returned `null`)
- `treesappTest > testQueryMichaelJackson() FAILED` (returned `null`)

#### Root Cause Analysis:
1. In `src/test/java/treesappTest.java`, the test setup executes:
   ```java
   map = new SimpleMapBST<>();
   SimpleMapBSTBuilder.readGenericMapFromCSV(map, "Top5000SongsEdited.csv");
   ```
2. The `datafiles/` directory in the initial assignment template contained smaller sample files (`Top10SongsEdited.csv`, `CityAndPopulation.csv`, `SongBoardInput.csv`, etc.), but was missing `Top5000SongsEdited.csv`.
3. Inspection of `codegrade-configORIG.txt` revealed the assignment submission requirement:
   ```text
   STUDENT_SUBMIT
   datafiles/Top5000SongsEdited.csv
   STUDENT_SUBMIT_END
   ```
   CodeGrade is configured to specifically accept and grade `datafiles/Top5000SongsEdited.csv` submitted by the student.

---

### 3. Actions Taken
1. **Located and Verified Dataset**:
   - Located the 5,000-song dataset from earlier modules (CSCI210-M14 and CSCI210-M15).
   - Confirmed the file contents: 5,001 lines total (1 header row `Musical Artist,Song Title` + 5,000 song entries).
   - Verified that the record line numbers and artist entries directly correspond to the exact assertions in `treesappTest.java`:
     - Line 10 (CSV line 11): `Elton John,Candle in the Wind '97`
     - Line 221 (CSV line 222): `Elton John,Crocodile Rock`
     - Line 4843 (CSV line 4844): `Elton John,Empty Garden (Hey Hey Johnny)`
     - Line 28 (CSV line 29): `Michael Jackson,Billie Jean`
     - Line 4804 (CSV line 4805): `Michael Jackson,Who is It`
2. **Placed File in Project Directory**:
   - Added `Top5000SongsEdited.csv` to `datafiles/Top5000SongsEdited.csv`.
3. **Environment Setup & Execution**:
   - Configured `JAVA_HOME` to point to the local OpenJDK installation (`C:\Users\bacon\.jdks\openjdk-26.0.2.1`).
   - Cleaned any build locks and executed Gradle build and test cycles with `--no-daemon`.

---

### 4. Technical Architecture
- **`SimpleMap<K, V>`**: The core interface specifying standard map operations: `V get(K key)`, `V put(K key, V value)`, and `V remove(K key)`.
- **`SimpleMapBST<K, V>`**: Implements `SimpleMap<K, V>` by wrapping an underlying `BinarySearchTree<Entry<K, V>>`. Handles collision by updating existing keys and appending/replacing associated values.
- **`BinarySearchTree<K, V>`**: Linked binary search tree handling generic key-value entries. Supports O(log n) average lookup, insertion, and deletion via successor substitution, as well as LeetCode 109 height-rebalancing (`rebalanceTreeL109`).
- **`SimpleMapBSTBuilder`**: Utility class providing `readGenericMapFromCSV` to read CSV headers and records into the BST map. Formats duplicate keys by aggregating lines into multi-line query responses (`Line <n>: <title>`).
- **`StringInt`**: A custom wrapper class implementing `Comparable<StringInt>` and `CharSequence` that utilizes `UtilityMethodsString.compareNumericStrings` to prioritize numerical comparison when strings represent numbers, falling back to lexicographical string comparison.
- **`UserQueryCSVDialog`**: Interactive Swing GUI for opening CSV datasets, viewing the visual tree topology with `SwingBinaryTreeDisplay`, and running real-time lookups.

---

### 5. Verification & Test Results
Ran Gradle test and build suites:
```bash
./gradlew.bat test --no-daemon
./gradlew.bat build --no-daemon
```
**Results:**
- `compileJava`: SUCCESS
- `compileTestJava`: SUCCESS
- `test`: SUCCESS (All JUnit test assertions passed: `testQueryEltonJohn`, `testQueryMichaelJackson`)
- `build`: SUCCESS (BUILD SUCCESSFUL)
