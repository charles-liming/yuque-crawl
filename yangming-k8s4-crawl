# BY @burpheart
# https://www.yuque.com/burpheart/phpaudit
# https://github.com/burpheart
import sys
import requests
import json
import re
import os
import urllib.parse
cookies = {
  "aliyungf_tc": "0e6ad2b64a32c95b983372005bd7821530c51b2ff8a411f0c4e9a47486df177d",
  "yuque_ctoken": "J7QemvmMLmjjzddPuEcfSNZE",
  "receive-cookie-deprecation": "1",
  "lang": "zh-cn",
  "_yuque_session": "Wp4jcnzLxQz_Ipaz8d5WOJpml5BFzBraLvRRdjSts-cILODZ4WmwiQdfPd0extmESSLEBDmDLHU-cR7sT1LLSw==",
  "_uab_collina": "172190028131965544945931",
  "tfstk": "f4jo_IXWKIG_7mMLZix7Rb2y8oUvFbtBP6np9HdUuIRjwbnRLkoFpObJ87tRxsffG_BRpMbjxO6C28tLF36WAHPT6lIhFTtQBVIxCg0q0pBq44JzPs4dUHPT6oHFAnB2YweiEHxc3I9HLp8FTjJ2GdvyYQ8EgqJwgB-eY68q3pvZa48EzEoV7Ok2w65507DFsN3dLDI2EUAnYKoHxio9lCWJi2lPmLAe_TRmYDA7Ph0cL6Pnvh71iMYwig-juVolsD94piuIRUJXnCg9Fi57wpv2E-2m7NTyhLrTn-0IeUJXnCe0nVlyzK9y6",
  "current_theme": "default",
  "acw_tc": "ac11000117219643112087066e10014e25f9a3d5952c6d1c44bf766a93839b"
}
def save_page(book_id, slug, path):
    url = f'https://www.yuque.com/api/docs/{slug}?book_id={book_id}&merge_dynamic_data=false&mode=markdown'
    response = requests.get(url,cookies=cookies)

    if response.status_code != 200:
        print(f"文档下载失败 页面可能被删除: {book_id} {slug} {response.content}")
        return

    try:
        docsjson = response.json()
        print(docsjson)
    except json.JSONDecodeError as e:
        print(f"Error decoding JSON response: {e}")
        return

    with open(path, 'w', encoding='utf-8') as f:
        f.write(docsjson['data']['sourcecode'])


def get_book(url="https://www.yuque.com/cnych/k8s4",cookies=cookies):
    response = requests.get(url,cookies=cookies)
    print(response)

    if response.status_code != 200:
        print(f"Error fetching URL {url}: {response.status_code}")
        return

    data = re.findall(r"decodeURIComponent\(\"(.+)\"\)\);", response.content.decode('utf-8'))

    if not data:
        print("Error extracting JSON data from the response")
        return

    try:
        docsjson = json.loads(urllib.parse.unquote(data[0]))
    except json.JSONDecodeError as e:
        print(f"Error decoding JSON data: {e}")
        return

    table = str.maketrans('\/:*?"<>|\n\r', "___________")

    book_id = docsjson.get('book', {}).get('id')
    if not book_id:
        print("Key 'book' not found in JSON response")
        return

    download_path = os.path.join("download", str(book_id))
    os.makedirs(download_path, exist_ok=True)

    list = {}
    temp = {}
    md = ""

    for doc in docsjson['book']['toc']:
        if doc['type'] == 'TITLE' or doc['child_uuid']:
            list[doc['uuid']] = {'title': doc['title'], 'parent_uuid': doc['parent_uuid']}
            temp[doc['uuid']] = build_path(list, doc['uuid'], table)

            full_path = os.path.join(download_path, temp[doc['uuid']])
            os.makedirs(full_path, exist_ok=True)

            if temp[doc['uuid']].endswith("/"):
                md += "## " + temp[doc['uuid']][:-1] + "\n"
            else:
                md += "  " * (temp[doc['uuid']].count("/") - 1) + "* " + temp[doc['uuid']].split("/")[-1] + "\n"

        if doc['url']:
            save_page_path = build_save_page_path(download_path, temp, doc, table)
            save_page(book_id, doc['url'], save_page_path)
            md += build_markdown_link(temp, doc, table) + "\n"

    with open(os.path.join(download_path, "SUMMARY.md"), 'w', encoding='utf-8') as f:
        f.write(md)


def build_path(list, uuid, table):
    path = ""
    while uuid:
        title = list[uuid]['title'].translate(table)
        path = title + '/' + path if path else title
        uuid = list[uuid]['parent_uuid']
    return path


def build_save_page_path(download_path, temp, doc, table):
    if doc['parent_uuid']:
        return os.path.join(download_path, temp[doc['parent_uuid']], doc['title'].translate(table) + '.md')
    return os.path.join(download_path, doc['title'].translate(table) + '.md')


def build_markdown_link(temp, doc, table):
    if doc['parent_uuid']:
        parent_path = temp[doc['parent_uuid']]
        indent = " " * parent_path.count("/")
        file_path = urllib.parse.quote(f"{parent_path}/{doc['title'].translate(table)}.md")
    else:
        indent = " "
        file_path = urllib.parse.quote(doc['title'].translate(table) + '.md')
    return f"{indent}* [{doc['title']}]({file_path})"


if __name__ == '__main__':
    if len(sys.argv) > 1:
        get_book(sys.argv[1])
    else:
        get_book()
