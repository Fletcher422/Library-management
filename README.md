import csv

class CommercialLibrarySystem:
    def __init__(self):
        self.students_db = {}
        self.max_books = 5

    def register_student(self, admission_no, name):
        """Helper to insert student into database if not present."""
        admission_no = str(admission_no).strip()
        name = name.strip()
        
        if admission_no and admission_no not in self.students_db:
            self.students_db[admission_no] = {
                "name": name,
                "books": []
            }

    def import_students_from_csv(self, file_path):
        """Reads a school spreadsheet and registers all students automatically."""
        try:
            with open(file_path, mode='r', encoding='utf-8') as file:
                # DictReader automatically uses the first row as column headers
                csv_reader = csv.DictReader(file)
                
                count = 0
                for row in csv_reader:
                    # Match the exact column names from the spreadsheet
                    adm_no = row['Admission Number']
                    name = row['Student Name']
                    
                    self.register_student(adm_no, name)
                    count += 1
                    
            print(f"🚀 Success: Onboarded {count} students into the library system database!")
            
        except FileNotFoundError:
            print(f"❌ Error: The file at {file_path} was not found. Check the path.")
        except KeyError:
            print("❌ Error: Spreadsheet column headers must match 'Admission Number' and 'Student Name'.")


