import json
import xml.etree.ElementTree as ET
data = []


with open("/content/test.json", "r") as f:
  data = json.load(f)
  print(data)

def get_xml_element_from_dict(data: dict, parent: ET.Element) -> ET.SubElement:
    for key, value in data.items():
        element = ET.SubElement(parent, key)
        if isinstance(value, dict):
            get_xml_element_from_dict(value, element)
        else:
            element.text = value


def get_xml_from_dict(data: dict) -> ET.ElementTree:
    root = ET.Element('Файл')
    get_xml_element_from_dict(data, root)
    tree = ET.ElementTree(root)
    return tree


def write_tree_to_file(tree: ET.ElementTree) -> None:
    with open('for_tests.xml', mode='wb') as file:
        tree.write(file, encoding='utf-8', xml_declaration=True)


result = get_xml_from_dict(data)
write_tree_to_file(result)
