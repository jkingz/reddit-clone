# Reddit Clone

## Router URLs

    /
        (recent posts from public and user's communities)
    /r/
        (recently created communities)
    /r/create/                                      :: check authentication
    /r/:communitySlug/                              :: check authentication
        (join or leave community)
    /r/:communitySlug/manage/                       :: check authentication and if the user is admin
        (edit, delete)
    /r/:communitySlug/posts/                        :: check authentication and membership or if the community is public
        (filter posts by :communitySlug)
    /r/:communitySlug/posts/create/                 :: check authentication and membership (membership is required for creating posts)
    /r/:communitySlug/posts/:postSlug/              :: check authentication and membership or if the community is public
        (detail view)
    /r/:communitySlug/posts/:postSlug/manage/       :: check authentication and if user is the post creator (or admin)
        (edit, delete)

    /user/:username/
    /user/:username/communities/
        (list user's communities)
    /user/:username/communities/
        (list user's public posts)
    /user/settings/                                 :: check authentication
        (change password, change email, verify email, add social account (FB, Google, Twitter), delete user account)


## `src/` Folder

    /components/
        CommunityDetailView.jsx
        CommunityManage.jsx
        CommunityListView.jsx
        PostDetailView.jsx
        PostManage.jsx
        PostListView.jsx
        Comment.jsx
        CommentSection.jsx
    /containers/
        CommunityDetailViewContainer.jsx
        CommunityManageContainer.jsx
        CommunityListViewContainer.jsx
        PostDetailViewContainer.jsx
        PostManageContainer.jsx
        PostListViewContainer.jsx
        CommentContainer.jsx
        CommentSectionContainer.jsx
    /pages/
        community.jsx
        landing.jsx
        posts.jsx 
            if /create/, PostManage[new] 
            if :communitySlug , then show PostDetailView
                if edit is present show PostManage[edit])
            otherwise show PostListView
    api.jsx
    App.jsx
    index.jsx
        (see API section)
    registerServiceWorker.js
    rourtes.jsx

Containers have `state` and handle API requests. They are also design-agnostic. Components are usually function based components and have desing elements.

## API

We shall consider use `ViewSet` for Community and Post CRUD and `django-filters` for filtering (communities by admin).

    /api/posts/ (list)
    /api/posts/post-slug-title/ (Post CRUD)
    
    /api/communities/ (list)
    /api/communities/community-slug-title/ (Community CRUD)
    
    /api/comments/ (list) (accepts ?community=community-slug and ?comment=comment-uuid)
    /api/comments/comment-slug-title/ (Comment CRUD)
    
    /rest-auth/user/  (GET, PUT, PATCH, other rest-auth use POST only)
    /rest-auth/user/  (DELETE(destroy): user.is_active = False)
    /rest-auth/registration/
    /rest-auth/registration/verify-email/
    /rest-auth/login/
    /rest-auth/logout/
    
    /rest-auth/password/reset/confirm/
    /rest-auth/password/reset/change/
    
    /rest-auth/facebook/
    /rest-auth/twitter/
    /rest-auth/google/

## Models (alleged)


    User
        first_name
        last_name
        email
        username
    
    Community
        title
        admin [FK User]
        is_public (boolean)
    
    Membership
        user [FK User]
        community [FK Community]
    
    Post
        title
        slug
        image
        text
        upvotes
        downvotes
        community [FK Community]
        creator [FK User]
    
    Comment
        text
        post [FK Post]
        comment [FK Comment] (default=None)
        user [FK User]
        upvotes
        downvotes
