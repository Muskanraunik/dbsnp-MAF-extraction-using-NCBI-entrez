import requests
import xml.etree.ElementTree as ET
import pandas as pd

def fetch_snp_data(rs_id):
    url = f"https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=snp&id={rs_id}&rettype=docset&retmode=text"
    response = requests.get(url)
    
    if response.status_code == 200:
        return response.text  # Return the SNP data as text
    else:
        return None
def extract_maf_values(xml_data):
    maf_values = []
    root = ET.fromstring(xml_data)  # Parse the XML data
    
    # Find all MAF elements in the XML
    for maf in root.findall('.//MAF'):
        study = maf.find('STUDY').text  # Get the study name
        freq = maf.find('FREQ').text  # Get the MAF frequency
        maf_values.append(f"Study: {study}, Frequency: {freq}")
    
    return maf_values

def main():
    input_csv = 'D:\\systematic_sclerosis_project\\m\\SNPdata\\Patients\\lifecell\\csv_files\\newapproach_final_patients\\result\\patient_specific_rs_id.csv'  # Replace with your CSV file path
    output_txt = 'maf_patient_specific_values.txt'  # Output text file
    
    df = pd.read_csv(input_csv)
    rs_ids = df['rs_id'].tolist()
    all_maf_values = []

    for rs_id in rs_ids:
        xml_data = fetch_snp_data(rs_id)  # Fetch the XML data
        if xml_data:
            maf_values = extract_maf_values(xml_data)  # Extract MAF values
            if maf_values:
                # Prepend the rs_id to each MAF value
                for maf in maf_values:
                    all_maf_values.append(f"{rs_id}: {maf}")  # Add rs_id to the MAF result
            else:
                all_maf_values.append(f"{rs_id}: No MAF values found.")
        else:
            all_maf_values.append(f"{rs_id}: Error fetching data.")
    
    with open(output_txt, 'w') as f:
        for maf in all_maf_values:
            f.write(maf + '\n')  # Write each MAF value on a new line
    
    print(f"MAF values saved to {output_txt}.")
if __name__ == "__main
