import os
import shutil
import re
from pathlib import Path

class FaithPhysicsOrganizer:
    def __init__(self, base_path):
        self.base_path = Path(base_path)
        
        # Define standard folder structure
        self.standard_structure = {
            "Academy": ["Case Studies", "Experiments", "Research Lab", "Teaching Materials"],
            "Meta": ["Connections", "Hypothesis", "Visualizations"],
            "Research": ["Integration Notes", "Scientific Papers", "Spiritual Texts"],
            "Concepts": ["Quantum Mechanics", "Information Theory", "Strong Nuclear Force"]
        }
        
        # Define content categories and their related terms
        self.content_categories = {
            "Quantum Mechanics": ["quantum", "entanglement", "superposition", "wave function", "collapse", "coherence", "decoherence"],
            "Information Theory": ["information", "entropy", "communication", "divine logos", "theological clarity", "meaning", "paradox"],
            "Strong Nuclear Force": ["nuclear force", "binding energy", "unity", "cohesion", "bond", "spiritual parallel"]
        }
        
        # Track processed files to avoid duplicates
        self.processed_files = set()
    
    def categorize_file(self, file_path):
        """Categorize a file based on its content."""
        # If file doesn't exist or is already processed, skip
        if not os.path.exists(file_path) or str(file_path) in self.processed_files:
            return None
        
        try:
            with open(file_path, 'r', encoding='utf-8') as f:
                content = f.read().lower()
                
                # Check each category
                for category, terms in self.content_categories.items():
                    for term in terms:
                        if term.lower() in content:
                            return category
                
                # If no specific category matches, use a general category
                if "quantum" in file_path.lower() or "quantum" in content:
                    return "Quantum Mechanics"
                elif "information" in file_path.lower() or "information" in content:
                    return "Information Theory"
                elif "nuclear" in file_path.lower() or "nuclear" in content or "unity" in content:
                    return "Strong Nuclear Force"
                
                # Default category
                return "General"
        except Exception as e:
            print(f"Error reading {file_path}: {e}")
            return None
    
    def create_directory_structure(self, law_folder):
        """Create standard directory structure for a Law folder if it doesn't exist."""
        law_path = self.base_path / law_folder
        
        # Create Concepts folder and subfolders
        for main_folder, sub_folders in self.standard_structure.items():
            main_path = law_path / main_folder
            main_path.mkdir(exist_ok=True, parents=True)
            
            for sub_folder in sub_folders:
                sub_path = main_path / sub_folder
                sub_path.mkdir(exist_ok=True, parents=True)
    
    def organize_law_folder(self, law_folder):
        """Organize a single Law folder."""
        print(f"\nOrganizing {law_folder}...")
        law_path = self.base_path / law_folder
        
        # Ensure directory structure exists
        self.create_directory_structure(law_folder)
        
        # Collect all markdown files in the law folder (including subdirectories)
        md_files = []
        for root, _, files in os.walk(law_path):
            for file in files:
                if file.endswith('.md') and not file == f"{law_folder} Index.md":
                    md_files.append(Path(root) / file)
        
        # First pass - identify main files to keep at root level
        root_files = []
        for file_path in md_files:
            file_name = file_path.name.lower()
            if file_name.startswith(law_folder.lower().replace(" ", "")) and "main" in file_name.lower():
                root_files.append(file_path)
            elif file_name == f"{law_folder.lower().replace(' ', '')}.md":
                root_files.append(file_path)
            elif file_name == f"{law_folder.lower().replace(' ', '')}-main-document.md":
                root_files.append(file_path)
            elif "story" in file_name.lower() and law_folder.lower().replace(" ", "") in file_name.lower():
                root_files.append(file_path)
        
        # Process non-root files
        for file_path in md_files:
            # Skip already processed files or root files
            if str(file_path) in self.processed_files or file_path in root_files:
                continue
            
            # Determine category
            category = self.categorize_file(file_path)
            
            if category and category in self.content_categories:
                # Move to appropriate Concepts subfolder
                target_dir = law_path / "Concepts" / category
                target_dir.mkdir(exist_ok=True, parents=True)
                
                # Create target path
                target_path = target_dir / file_path.name
                
                # Handle duplicate filename
                if target_path.exists():
                    # If target already exists with same content, skip this file
                    try:
                        with open(file_path, 'r', encoding='utf-8') as f1, open(target_path, 'r', encoding='utf-8') as f2:
                            if f1.read() == f2.read():
                                self.processed_files.add(str(file_path))
                                continue
                    except Exception:
                        pass
                    
                    # If content differs, rename to avoid conflict
                    base_name = target_path.stem
                    counter = 1
                    while target_path.exists():
                        target_path = target_dir / f"{base_name}_{counter}{target_path.suffix}"
                        counter += 1
                
                try:
                    # Move file
                    print(f"Moving {file_path.name} to Concepts/{category}/")
                    shutil.move(str(file_path), str(target_path))
                    self.processed_files.add(str(file_path))
                except Exception as e:
                    print(f"Error moving {file_path}: {e}")
            else:
                # For general non-categorized files, move to Meta folder if not already there
                rel_path = file_path.relative_to(law_path) if file_path.is_relative_to(law_path) else file_path.name
                parent_dir = str(rel_path.parent).lower()
                
                if "academy" in parent_dir:
                    continue  # Keep files already in Academy folder
                elif "research" in parent_dir:
                    continue  # Keep files already in Research folder
                elif "meta" in parent_dir:
                    continue  # Keep files already in Meta folder
                
                # For files in root or other non-standard locations, move to Meta/Connections
                if "connect" in file_path.name.lower() or "relation" in file_path.name.lower():
                    target_dir = law_path / "Meta" / "Connections"
                elif "hypoth" in file_path.name.lower() or "theory" in file_path.name.lower():
                    target_dir = law_path / "Meta" / "Hypothesis"
                else:
                    target_dir = law_path / "Meta" / "Connections"
                
                target_path = target_dir / file_path.name
                
                # Handle duplicate filename
                if target_path.exists():
                    base_name = target_path.stem
                    counter = 1
                    while target_path.exists():
                        target_path = target_dir / f"{base_name}_{counter}{target_path.suffix}"
                        counter += 1
                
                try:
                    # Move file
                    print(f"Moving {file_path.name} to Meta folder")
                    shutil.move(str(file_path), str(target_path))
                    self.processed_files.add(str(file_path))
                except Exception as e:
                    print(f"Error moving {file_path}: {e}")
    
    def create_index_file(self, law_folder):
        """Create an index file for a Law folder."""
        law_path = self.base_path / law_folder
        
        # Get all markdown files in the Law folder, organized by directory
        files_by_dir = {}
        for root, _, files in os.walk(law_path):
            rel_root = os.path.relpath(root, law_path)
            if rel_root == '.':
                rel_root = 'Root'
            
            md_files = [f for f in files if f.endswith('.md') and f != f"{law_folder} Index.md"]
            if md_files:
                files_by_dir[rel_root] = md_files
        
        # Prepare index content
        law_num = re.search(r'Law (\d+)', law_folder)
        law_title = ""
        
        # Try to find a title from a main document
        main_doc = law_path / f"{law_folder} - Main Document.md"
        if main_doc.exists():
            try:
                with open(main_doc, 'r', encoding='utf-8') as f:
                    content = f.read()
                    title_match = re.search(r'# (.*)', content)
                    if title_match:
                        law_title = title_match.group(1)
            except Exception:
                pass
        
        if not law_title:
            # Use a generic title based on folder name
            law_title = f"{law_folder}"
        
        index_content = f"# {law_title}\n\n"
        
        # Add Core Concepts section if exists
        if 'Concepts' in files_by_dir or any(d.startswith('Concepts/') for d in files_by_dir):
            index_content += "## Core Concepts\n\n"
            
            for concept_type in self.content_categories:
                concept_dir = f"Concepts/{concept_type}"
                if concept_dir in files_by_dir and files_by_dir[concept_dir]:
                    index_content += f"### {concept_type}\n\n"
                    for file in sorted(files_by_dir[concept_dir]):
                        link_text = file.replace('.md', '')
                        link_path = f"{concept_dir}/{file}"
                        index_content += f"- [{link_text}]({link_path})\n"
                    index_content += "\n"
        
        # Add Research Materials section if exists
        if 'Research' in files_by_dir or any(d.startswith('Research/') for d in files_by_dir):
            index_content += "## Research Materials\n\n"
            
            for research_type in ['Integration Notes', 'Scientific Papers', 'Spiritual Texts']:
                research_dir = f"Research/{research_type}"
                if research_dir in files_by_dir and files_by_dir[research_dir]:
                    index_content += f"### {research_type}\n\n"
                    for file in sorted(files_by_dir[research_dir]):
                        link_text = file.replace('.md', '')
                        link_path = f"{research_dir}/{file}"
                        index_content += f"- [{link_text}]({link_path})\n"
                    index_content += "\n"
        
        # Add Meta Analysis section if exists
        if 'Meta' in files_by_dir or any(d.startswith('Meta/') for d in files_by_dir):
            index_content += "## Meta Analysis\n\n"
            
            for meta_type in ['Connections', 'Hypothesis', 'Visualizations']:
                meta_dir = f"Meta/{meta_type}"
                if meta_dir in files_by_dir and files_by_dir[meta_dir]:
                    index_content += f"### {meta_type}\n\n"
                    for file in sorted(files_by_dir[meta_dir]):
                        link_text = file.replace('.md', '')
                        link_path = f"{meta_dir}/{file}"
                        index_content += f"- [{link_text}]({link_path})\n"
                    index_content += "\n"
        
        # Add Academy Resources section if exists
        if 'Academy' in files_by_dir or any(d.startswith('Academy/') for d in files_by_dir):
            index_content += "## Academy Resources\n\n"
            
            for academy_type in ['Teaching Materials', 'Case Studies', 'Experiments', 'Research Lab']:
                academy_dir = f"Academy/{academy_type}"
                if academy_dir in files_by_dir and files_by_dir[academy_type]:
                    index_content += f"### {academy_type}\n\n"
                    for file in sorted(files_by_dir[academy_type]):
                        link_text = file.replace('.md', '')
                        link_path = f"{academy_dir}/{file}"
                        index_content += f"- [{link_text}]({link_path})\n"
                    index_content += "\n"
        
        # Add Main Documents section for root files
        if 'Root' in files_by_dir and files_by_dir['Root']:
            index_content += "## Main Documents\n\n"
            for file in sorted(files_by_dir['Root']):
                link_text = file.replace('.md', '')
                link_path = file
                index_content += f"- [{link_text}]({link_path})\n"
            index_content += "\n"
        
        # Write index file
        index_path = law_path / f"{law_folder} Index.md"
        print(f"Creating index file: {index_path}")
        with open(index_path, "w", encoding="utf-8") as f:
            f.write(index_content)
    
    def create_master_index(self):
        """Create a master index file linking to all Law folders."""
        master_index_content = "# Faith with Physics - Master Index\n\n"
        master_index_content += "## Universal Laws\n\n"
        
        # Get all Law folders, sort by number
        law_folders = [d for d in os.listdir(self.base_path) 
                      if os.path.isdir(os.path.join(self.base_path, d)) 
                      and d.startswith("Law")]
        
        # Sort Law folders by number
        law_folders.sort(key=lambda x: int(re.search(r'Law (\d+)', x).group(1)) 
                        if re.search(r'Law (\d+)', x) else 0)
        
        # Add links to each Law folder
        for law_folder in law_folders:
            master_index_content += f"- [{law_folder}]({law_folder}/{law_folder} Index.md)\n"
        
        # Add links to other special folders
        master_index_content += "\n## Additional Resources\n\n"
        
        if os.path.isdir(os.path.join(self.base_path, "Glossary")):
            master_index_content += "- [Glossary](Glossary/Glossary.md)\n"
        
        # Write master index file
        master_index_path = self.base_path / "Faith with Physics - Master Index.md"
        print(f"Creating master index file: {master_index_path}")
        with open(master_index_path, "w", encoding="utf-8") as f:
            f.write(master_index_content)
    
    def organize_all_laws(self):
        """Organize all Law folders in the base path."""
        # Get all Law folders
        law_folders = [d for d in os.listdir(self.base_path) 
                      if os.path.isdir(os.path.join(self.base_path, d)) 
                      and d.startswith("Law")]
        
        # Sort Law folders by number
        law_folders.sort(key=lambda x: int(re.search(r'Law (\d+)', x).group(1)) 
                        if re.search(r'Law (\d+)', x) else 0)
        
        # Process each Law folder
        for law_folder in law_folders:
            self.organize_law_folder(law_folder)
            self.create_index_file(law_folder)
        
        # Create master index
        self.create_master_index()
        
        print("\nOrganization complete!")

def main():
    # Get the base path from user
    base_path = input("Enter the path to your Faith with Physics folder: ")
    
    # Create organizer and process all Law folders
    organizer = FaithPhysicsOrganizer(base_path)
    organizer.organize_all_laws()
    
    print("\nReorganization complete!")
    print("Some manual review may still be needed:")
    print("1. Check for any duplicate files that weren't moved correctly")
    print("2. Review the organization to ensure it meets your needs")
    print("3. Update any internal links or references in your markdown files")

if __name__ == "__main__":
    main()