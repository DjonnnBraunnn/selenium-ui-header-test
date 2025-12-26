from selenium import webdriver
from time import sleep


def test_header_elements():
    """Проверяем, что в шапке есть нужные пункты меню и переключатели языка."""
    driver = webdriver.Firefox()
    driver.maximize_window()

    try:
        driver.get("https://itcareerhub.de/ru")
        sleep(3)  # даём странице загрузиться

        page_source = driver.page_source

        # Логотип / название
        assert "ITCareerHub" in page_source or "IT Career Hub" in page_source, "Logo text not found"

        # Пункты меню
        menu_items = ["Программы", "Способы оплаты", "Новости", "О нас", "Отзывы"]
        for item in menu_items:
            assert item in page_source, f"Menu item '{item}' not found in page source"

        # Переключатели языка
        assert "ru" in page_source, "Language toggle 'ru' not found"
        assert "de" in page_source, "Language toggle 'de' not found"
    finally:
        driver.quit()


def test_phone_icon_message():
    """Проверяем, что текст про звонок и форму присутствует на странице."""
    driver = webdriver.Firefox()
    driver.maximize_window()

    try:
        driver.get("https://itcareerhub.de/ru")
        sleep(3)

        page_source = driver.page_source

        expected_text = (
            "Если вы не дозвонились, заполните форму на сайте. Мы свяжемся с вами"
        )

        assert expected_text in page_source, "Expected phone info message not found in page HTML"
    finally:
        driver.quit()