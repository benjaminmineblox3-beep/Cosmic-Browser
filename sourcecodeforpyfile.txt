import sys
from PyQt6.QtCore import QUrl
from PyQt6.QtWidgets import QApplication, QMainWindow, QVBoxLayout, QHBoxLayout, QWidget, QLineEdit, QPushButton
from PyQt6.QtWebEngineCore import QWebEnginePage
from PyQt6.QtWebEngineWidgets import QWebEngineView
from PyQt6.QtWidgets import QWidgets, QPushButton, QLineEdit

class CosmicBrowser(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Cosmic Browser")
        self.resize(1024, 768)

        # here are widgets and layouts lol
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        main_layout = QVBoxLayout(central_widget)
        main_layout.setContentsMargins(0, 0, 0, 0)
        main_layout.setSpacing(0)

        # Toolbar layout here
        toolbar_layout = QHBoxLayout()
        main_layout.addLayout(toolbar_layout)

        # Navigation btns here
        self.back_btn = QPushButton("←")
        self.forward_btn = QPushButton("→")
        self.reload_btn = QPushButton("⟳")
        toolbar_layout.addWidget(self.back_btn)
        toolbar_layout.addWidget(self.forward_btn)
        toolbar_layout.addWidget(self.reload_btn)

        # Address bar
        self.address_bar = QLineEdit()
        toolbar_layout.addWidget(self.address_bar)

        # Chromium web view
        self.web_view = QWebEngineView()
        main_layout.addWidget(self.web_view)

        # connecting ui
        self.address_bar.returnPressed.connect(self.navigate_to_url)
        self.back_btn.clicked.connect(self.web_view.back)
        self.forward_btn.clicked.connect(self.web_view.forward)
        self.reload_btn.clicked.connect(self.web_view.reload)
        self.web_view.urlChanged.connect(self.update_address_bar)

        # load initial homepage
        self.web_view.load(QUrl("https://google.com"))

    def navigate_to_url(self):
        text = self.address_bar.text()
        # idk how to describe
        if not text.startswith("http://") and not text.startswith("https://"):
            text = "https://" + text
        self.web_view.load(QUrl(text))

    def update_address_bar(self, url):
        self.address_bar.setText(url.toString())

if __name__ == "__main__":
    app = QApplication(sys.argv)
    browser = CosmicBrowser()
    browser.show()
    sys.exit(app.exec())

    print("loaded")
