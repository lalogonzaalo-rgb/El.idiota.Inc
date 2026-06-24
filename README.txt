client = genai.Client()

    chat = client.chats.create(
        model="gemini-3-flash-preview",
        history=[
            types.Content(
                role="user", parts=[types.Part(text="Hi my name is Bob")]
            ),
            types.Content(role="model", parts=[types.Part(text="Hi Bob!")]),
        ],
    )

    print(
        client.models.count_tokens(
            model="gemini-3-flash-preview", contents=chat.get_history()
        )
    )

    response = chat.send_message(
        message="In one sentence, explain how a computer works to a young child."
    )
    print(response.usage_metadata)

    extra = types.UserContent(
        parts=[
            types.Part(
                text="What is the meaning of life?",
            )
        ]
    )
    history = [*chat.get_history(), extra]
    print(client.models.count_tokens(model="gemini-3-flash-preview", contents=history))

