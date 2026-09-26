CATEGORIES = [
    "텍스트 생성",
    "이미지 생성",
    "영상 생성",
    "페르소나",
    "자동화",
    "기타"
]


prompts = [
    {
        "title": "블로그 글 작성 도우미",
        "content": (
            "당신은 10년 경력의 전문 블로거입니다.\n"
            "주어진 주제에 대해 SEO에 최적화된 블로그 글을 작성해주세요.\n"
            "서론, 본론, 결론 구조를 갖추고 독자의 관심을 끄는 제목을 3개 제안해주세요."
        ),
        "category": "텍스트 생성",
        "favorite": True
    },
    {
        "title": "제품 썸네일 생성",
        "content": (
            "다음 제품의 특징을 분석하고 구매자의 관심을 끌 수 있는 "
            "매력적인 썸네일 이미지 생성 프롬프트를 작성해주세요."
        ),
        "category": "이미지 생성",
        "favorite": False
    },
    {
        "title": "IT 컨설턴트 페르소나",
        "content": (
            "당신은 15년 경력의 IT 컨설턴트입니다.\n"
            "복잡한 기술 내용을 비전공자도 이해할 수 있도록 쉽고 명확하게 설명해주세요."
        ),
        "category": "페르소나",
        "favorite": False
    }
]


def show_menu():
    """메인 메뉴를 출력한다."""
    print("\n" + "=" * 40)
    print("       나만의 프롬프트 관리")
    print("=" * 40)
    print("1. 프롬프트 추가")
    print("2. 프롬프트 목록")
    print("3. 카테고리별 조회")
    print("4. 프롬프트 검색")
    print("5. 프롬프트 상세 보기")
    print("6. 즐겨찾기 관리")
    print("7. 즐겨찾기 목록")
    print("0. 종료")
    print("=" * 40)


def show_categories():
    """카테고리 목록을 출력한다."""
    print("\n카테고리 선택")

    for index, category in enumerate(CATEGORIES, start=1):
        print(f"{index}) {category}")

    print(f"{len(CATEGORIES) + 1}) 직접 입력")


def select_category():
    """사용자로부터 카테고리를 입력받는다."""
    while True:
        show_categories()

        choice = input("선택: ").strip()

        if choice.isdigit():
            number = int(choice)

            if 1 <= number <= len(CATEGORIES):
                return CATEGORIES[number - 1]

            if number == len(CATEGORIES) + 1:
                while True:
                    custom_category = input("카테고리 이름: ").strip()

                    if custom_category:
                        return custom_category

                    print("카테고리를 비워둘 수 없습니다.")

        print("올바른 카테고리를 선택해주세요.")


def get_non_empty_input(message):
    """빈 문자열이 입력되지 않도록 확인한다."""
    while True:
        value = input(message).strip()

        if value:
            return value

        print("입력값이 비어있습니다. 다시 입력해주세요.")


def add_prompt():
    """새로운 프롬프트를 추가한다."""
    print("\n=== 프롬프트 추가 ===")

    title = get_non_empty_input("제목: ")
    content = get_non_empty_input("내용: ")
    category = select_category()

    new_prompt = {
        "title": title,
        "content": content,
        "category": category,
        "favorite": False
    }

    prompts.append(new_prompt)

    print("\n프롬프트가 추가되었습니다!")


def print_prompt_summary(index, prompt):
    """프롬프트 한 개의 요약 정보를 출력한다."""
    favorite_mark = " ⭐" if prompt["favorite"] else ""

    print(
        f"{index}. "
        f"[{prompt['category']}] "
        f"{prompt['title']}"
        f"{favorite_mark}"
    )


def show_list(prompt_list=None):
    """전체 프롬프트 목록을 출력한다."""
    if prompt_list is None:
        prompt_list = prompts

    print("\n=== 프롬프트 목록 ===")

    if not prompt_list:
        print("등록된 프롬프트가 없습니다.")
        return

    for index, prompt in enumerate(prompt_list, start=1):
        print_prompt_summary(index, prompt)

    print(f"\n총 {len(prompt_list)}개의 프롬프트")


def show_by_category():
    """카테고리별 프롬프트를 출력한다."""
    print("\n=== 카테고리별 조회 ===")

    category = select_category()

    filtered_prompts = [
        prompt for prompt in prompts
        if prompt["category"] == category
    ]

    print(f"\n[{category}] 카테고리 프롬프트:")

    if not filtered_prompts:
        print("해당 카테고리에 등록된 프롬프트가 없습니다.")
        return

    for index, prompt in enumerate(filtered_prompts, start=1):
        print_prompt_summary(index, prompt)

    print(f"\n총 {len(filtered_prompts)}개의 프롬프트")


def search_prompt():
    """제목 또는 내용에서 키워드를 검색한다."""
    print("\n=== 프롬프트 검색 ===")

    keyword = get_non_empty_input("검색어: ").lower()

    results = []

    for prompt in prompts:
        title = prompt["title"].lower()
        content = prompt["content"].lower()

        if keyword in title or keyword in content:
            results.append(prompt)

    if not results:
        print("\n검색 결과가 없습니다.")
        return

    print("\n검색 결과:")

    for index, prompt in enumerate(results, start=1):
        print_prompt_summary(index, prompt)

    print(f"\n{len(results)}개의 프롬프트를 찾았습니다.")


def get_prompt_number():
    """프롬프트 번호를 입력받고 유효성을 검사한다."""
    if not prompts:
        print("등록된 프롬프트가 없습니다.")
        return None

    value = input("프롬프트 번호 입력: ").strip()

    if not value.isdigit():
        print("번호를 숫자로 입력해주세요.")
        return None

    number = int(value)

    if number < 1 or number > len(prompts):
        print("존재하지 않는 프롬프트 번호입니다.")
        return None

    return number - 1


def show_detail():
    """선택한 프롬프트의 상세 정보를 출력한다."""
    print("\n=== 프롬프트 상세 보기 ===")

    index = get_prompt_number()

    if index is None:
        return

    prompt = prompts[index]

    favorite_mark = "⭐" if prompt["favorite"] else "즐겨찾기 아님"

    print("\n" + "─" * 40)
    print(f"제목: {prompt['title']}")
    print(f"카테고리: {prompt['category']}")
    print(f"즐겨찾기: {favorite_mark}")
    print("─" * 40)
    print("내용:")
    print(prompt["content"])
    print("─" * 40)


def manage_favorite():
    """프롬프트의 즐겨찾기 상태를 변경한다."""
    print("\n=== 즐겨찾기 관리 ===")

    index = get_prompt_number()

    if index is None:
        return

    prompt = prompts[index]

    if prompt["favorite"]:
        prompt["favorite"] = False
        print(f"'{prompt['title']}' 프롬프트를 즐겨찾기에서 해제했습니다.")
    else:
        prompt["favorite"] = True
        print(f"'{prompt['title']}' 프롬프트를 즐겨찾기에 추가했습니다!")


def show_favorites():
    """즐겨찾기된 프롬프트만 출력한다."""
    print("\n=== 즐겨찾기 목록 ===")

    favorite_prompts = [
        prompt for prompt in prompts
        if prompt["favorite"]
    ]

    if not favorite_prompts:
        print("즐겨찾기한 프롬프트가 없습니다.")
        return

    for index, prompt in enumerate(favorite_prompts, start=1):
        print_prompt_summary(index, prompt)

    print(f"\n총 {len(favorite_prompts)}개의 즐겨찾기")


def main():
    """프로그램의 메인 실행 함수."""
    while True:
        show_menu()

        choice = input("선택: ").strip()

        if choice == "1":
            add_prompt()

        elif choice == "2":
            show_list()

        elif choice == "3":
            show_by_category()

        elif choice == "4":
            search_prompt()

        elif choice == "5":
            show_detail()

        elif choice == "6":
            manage_favorite()

        elif choice == "7":
            show_favorites()

        elif choice == "0":
            print("\n프로그램을 종료합니다. 감사합니다!")
            break

        else:
            print("\n잘못된 메뉴 번호입니다.")
            print("0~7 사이의 번호를 입력해주세요.")


if __name__ == "__main__":
    main()
